+++
title = "OAuth Tutorial with Go and the Spotify API"
date = "2024-10-30"
author = "Ukeje Goodness"
description = "This article will help you understand OAuth2 so that you can build and integrate external services that allow users to sign up and sign in with their external profiles, such as GitHub, Spotify, Google, e.t.c"

[taxonomies]
tags = ["Go", "Authentication", "Security", "Integration"]
+++

---

When OAuth was released, it solved a major issue with application security, privacy, and user experience by allowing
builders access to user data from external apps. OAuth2 has complexities that you’ll need to understand at least at a
high level to integrate external services requiring OAuth2 in your apps.

This article will help you understand OAuth2 so that you can build and integrate external services that allow users to
sign up and sign in with their external profiles, such as GitHub, Spotify, Google, etc.

## **Understanding the OAuth Flow**

![OAuth Flow](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/l6jj1prahmrtnyoemcnb.png)

OAuth has a set of key components, parameters, and a different flow you’ll need to understand before building; let’s
review them.

### OAuth Concepts and Parameters

- The **Authorization Server** handles user authentication and grants permissions to the client, in this case, Spotify.
- The **Resource Server** is the server that hosts the data you want to access from the provider, in this case,
  Spotify’s API endpoints.
- The **Client** is the app you’re building to make requests to the authorization and resource servers.
- The **Resource Owner** owns the data you need to access from the OAuth provider.
- The OAuth provider issues a **Client ID and Client Secret** to identify your app. The **Client ID** identifies your
  app, while the **Client Secret** is for authentication.
- The **Authorization Code** is a short-lived code that the authorization server issues the client.
- The authorization server also issues an **Access Token** for your client to access the resource.
- The **Refresh Token** is long-lived, so your client can get new tokens from the authorization server when they expire
  without the user needing to authenticate again.
- You’ll provide a **Redirect URI**  to the OAuth provider so the authorization server can redirect the resource owner
  after they’ve authenticated and you’re authorized or they reject the request.
- You’ll specify the **`Scopes`** in your request to define the resources you want to access. The OAuth provider would
  notify the resource owner of the scopes before they authenticate.
- You must include a **State Parameter** in your request to maintain the state between the authorization request and
  callback. This step helps prevent **Cross-Site Request Forgery (CSRF)** attacks.

### The OAuth2 Authorization Flow

1. When the user tries to give you access to an external resource, you need to direct them to the authorization server
   with a request that includes the parameters to indicate the data you need from the resource server.
2. The user reserves the right to grant or deny access. When they do, the authorization server will redirect them to the
   redirect URI with or without the authorization code.
3. If the user grants the client access, the client sends the authorization code and credentials to the authorization
   server in exchange for an access token.
4. Now, you can use the Access Token to access the user’s resource on the resource server.

## Setup and Prerequisites

This article requires basic knowledge of Go, but it can also be read in other languages.

Since we are using the Spotify API for demonstration, you must [set up an account](https://developer.spotify.com/) to
retrieve a Client ID and Client Secret for authorization. Remember to set the callback address to which Spotify will
redirect users.

![Spotify Dashboard](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9jqiug26stpw9xdvjr0m.png)

Now, initialize a new project and install the `oauth2` package you’ll use for authorization.

```bash
go mod init
go get golang.org/x/oauth2
```

Finally, import these packages into your `main.go` file. Aside from the `oauth2` package, every package on the list is
part of the Go standard library.

```go
import (
	"context"
	"encoding/json"
	"fmt"
	"golang.org/x/oauth2"
	"golang.org/x/oauth2/spotify"
	"log"
	"math/rand"
	"net/http"
	"time"
)
```

Before you start authorization with Oauth, you’ll need to understand how it all works; let’s get to it.

## **Implementing OAuth in Go**

![OAuth Endpoint](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ozomr1iooqoce3stjomq.png)

Now that you’re all set up, define a struct for the OAuth service. When creating a new service, you’ll initialize OAuth
configurations with `*oauth2.Config`.

```go
type OAuthService struct {
	config *oauth2.Config
	state  string
}
```

The `state` variable will be the state parameter; you’ll have something secure that you can always verify.

You can use the `rand.Seed` function to seed a random 64-it integer randomized by the current time.

```go
func generateRandomState() string {
	rand.Seed(time.Now().UnixNano())
	return fmt.Sprintf("%d", rand.Intn(100000)) 
}
```

Calling `rand.Intn(100000)` sets the upper limit of the generated random number.

Now, you can define a function that will initialize a new OAuth service using the necessary parameters.

```go
func NewOAuthService() *OAuthService {
	return &OAuthService{
		config: &oauth2.Config{
			ClientID:     "your client id",
			ClientSecret: "<your client secret",
			Endpoint:     spotify.Endpoint,
			RedirectURL:  "http://localhost:8080/callback/spotify",
			Scopes:       []string{"user-read-email", "user-read-recently-played"},
		},
		state: generateRandomState(), // Use a dynamic state in production
	}
}
```

It’s important to enter the right `RedirectURL` you provided to the OAuth provider and define only the necessary scopes.

You’ll need a login function that directs the users to the OAuth provider to initiate the OAuth process.

```go
func (o *OAuthService) HandleSpotifyLogin(w http.ResponseWriter, r *http.Request) {
	url := o.config.AuthCodeURL(o.state)
	http.Redirect(w, r, url, http.StatusTemporaryRedirect)
}
```

Here, `AuthCodeURL` generates the URL and appends the state parameter before temporarily redirecting the user.

Now, you’ll need a function that handles callbacks from the OAuth provider. After extracting and validating state
parameters, the function should redirect users to your app.

```go
func (o *OAuthService) HandleCallback(w http.ResponseWriter, r *http.Request) {
	state := r.URL.Query().Get("state")
	code := r.URL.Query().Get("code")
	if state != o.state {
		http.Error(w, "Invalid state", http.StatusBadRequest)
		return
	}

	token, err := o.config.Exchange(context.Background(), code)
	if err != nil {
		http.Error(w, "Failed to exchange token", http.StatusInternalServerError)
		log.Println("Token exchange error:", err)
		return
	}

	client := o.config.Client(context.Background(), token)
	userInfo, err := getSpotifyUserInfo(client)
	if err != nil {
		http.Error(w, "Failed to get user info", http.StatusInternalServerError)
		log.Println("User info error:", err)
		return
	}
	fmt.Fprintf(w, "Logged in successfully! User: %s\n", userInfo.Name)

	playlistID := "37i9dQZF1EVHGWrwldPRtj" // Replace with the actual playlist ID you want to fetch
	tracks, err := getPlaylistTracks(client, playlistID)
	if err != nil {
		http.Error(w, "Failed to get playlist tracks", http.StatusInternalServerError)
		log.Println("Playlist tracks error:", err)
		return
	}

	for _, trackItem := range tracks {
		fmt.Fprintf(w, "Track: %s by %s (Album: %s)\n", trackItem.Track.Name, trackItem.Track.Artists[0].Name, trackItem.Track.Album.Name)
	}
}
```

First, the `HandleCallback` extracts the `code` and `state` parameters and validates the state. The `Exchange` method
requests for an access token with the `code` parameter.

Finally, the `Client` function returns an HTTP client with the access token. The token will auto-refresh as necessary.
You can now request the OAuth provider API from the client instance, as I have done with the `getSpotifyUserInfo`
function call.

```go
func getSpotifyUserInfo(client *http.Client) (string, error) {
	resp, err := client.Get("https://api.spotify.com/v1/me")
	if err != nil {
		return "", fmt.Errorf("failed to get user info: %w", err)
	}
	defer resp.Body.Close()

	if resp.StatusCode != http.StatusOK {
		return "", fmt.Errorf("error response from Spotify: %s", resp.Status)
	}

	var user struct {
		Name string `json:"display_name"`
	}
	if err := json.NewDecoder(resp.Body).Decode(&user); err != nil {
		return "", fmt.Errorf("failed to decode user info: %w", err)
	}

	return user.Name, nil
}
```

The `getSpotifyUserInfo` function accepts an HTTP client instance and makes a `GET` request to a Spotify API endpoint
that requires OAuth for access. The client from the `HandleCallback` function is the sauce that makes the request valid.

Finally, you need to call the OAuth service, register the routes and start a server.

```go
func main() {
	oauthService := NewOAuthService()

	http.HandleFunc("/login/spotify", oauthService.HandleSpotifyLogin)
	http.HandleFunc("/callback/spotify", oauthService.HandleCallback)

	fmt.Println("Server is running on http://localhost:8080")
	log.Fatal(http.ListenAndServe(":8080", nil))
}
```

When running this program and following through with the authentication process, you should get a message with your
username displayed on the browser.

![Browser Output](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hget484xwxrdk7smskh3.png)

## Best Practices to Consider

When implementing an OAuth provider, follow this safety and great UX practices.

- Use and enforce HTTPS to protect tokens from attackers.
- Use secure storage solutions if you’re storing tokens.
- Implement token expiration and automatic refreshes for good UX.
- Minimize scopes and permissions to what’s necessary.
- Do not store tokens in local storage for web apps.
- Rotate, and Invalidate Tokens When compromised or the user has logged out.
- Implement CSRF Protection with state parameters.

The data you get from OAuth providers is sensitive, so they specifically require OAuth on those endpoints. Security
first!

## Conclusion

Now that you know the basics of OAuth and how to implement OAuth providers and best practices, the next step is to build
the other parts of your apps.

Use this foundation to explore other features like refresh tokens for sessions with longer lives, and always consider
the best practices. Happy building.