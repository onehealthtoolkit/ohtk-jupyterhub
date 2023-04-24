# Links
- JupyterHub [https://jupyterhub.readthedocs.io/en/stable/index.html]
- Django OAuth Toolkit [https://github.com/jazzband/django-oauth-toolkit]

# Diagram
```
┌──────────────────────────────┐                             ┌─────────────────────────────────────────┐
│           ohtk-api           │                             │               JupyterHub                │
│                              │                             │                                         │
│                              │                             │                                         │
│       ┌────────────────┐     │                             │        ┌────────────────┐               │
│       │     Django     │     │                             │        │                │               │
│       │     OAuth      │◀────┼─────────────────────────────┼────────│  OAuth Client  │               │
│       │    Toolkit     │     │                             │        │                │               │
│       └────────────────┘     │                             │        └────────────────┘               │
│                              │                             │                 │  spawn                │
│                              │                             │                 │  docker               │
│                              │                             │           ┌─────┴────────────┐          │
│                              │                             │           │                  │          │
│                              │                             │           ▼                  ▼          │
│       ┌────────────────┐     │                             │  ┌────────────────┐ ┌────────────────┐  │
│       │   protected    │     │                             │  │    Jupyter     │ │    Jupyter     │  │
│       │   Export Api   │◀────┼─────────────────────────────┼──│    Notebook    │ │    Notebook    │  │
│       │                │     │                             │  │   (per user)   │ │   (per user)   │  │
│       └────────────────┘     │         access              │  └────────────────┘ └────────────────┘  │
│                              │                             │                                         │
└──────────────────────────────┘                             └─────────────────────────────────────────┘
```

# Multi Tenant
By using MultiOAuthenticator, Each tenant will have dedecated login button [using code from https://github.com/jupyterhub/oauthenticator/issues/136 ]

#### Configuration Example
```python
c.MultiOAuthenticator.authenticators = [
    (
        GenericOAuthenticator,
        "/bunny",
        {
            "client_id": "dCipgip8oPofpBNBy9mSKe8wei9pfrMdacXYU8eK",
            "client_secret": "test",
            "oauth_callback_url": "http://192.168.1.102:8888/hub/bunny/oauth_callback",
            "login_service": "bunny",
            "userdata_url": "https://bunny.opensur.test/api/userinfo/",
            "token_url": "https://bunny.opensur.test/o/token/",
            "authorize_url": "https://bunny.opensur.test/o/authorize/",
            "extra_authorize_params": {},
            "extra_params": {
                "scope": "user_info",
            },
        },
    ),
    (
        GenericOAuthenticator,
        "/bon",
        {
            "client_id": "dCipgip8oPofpBNBy9mSKe8wei9pfrMdacXYU8eK",
            "client_secret": "test",
            "oauth_callback_url": "http://192.168.1.102:8888/hub/bon/oauth_callback",
            "login_service": "Bon",
            "userdata_url": "https://bon.backend.ohtk.org/api/userinfo/",
            "token_url": "https://bon.backend.ohtk.org/o/token/",
            "authorize_url": "https://bon.backend.ohtk.org/o/authorize/",
            "extra_authorize_params": {},
            "extra_params": {
                "scope": "user_info",
            },
        },
    ),
]
c.JupyterHub.authenticator_class = MultiOAuthenticator

```

# Access Ohtk Api


# Challenges
- Duplicate username
  - username format -> '{tenant_name}_{username}'
- How to access ohtk api endpoint
  - use Basic Autheniticate rest api 