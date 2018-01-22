Garlic Authorization bundle
=====================

## JWT token authorization bundle

### Installation

#### 1. Run:

```bash
$ composer require garlic/auth
```

#### 2. Add to .env.dist (.env):

```bash
# Required
JWT_PUBLIC_KEY_PATH='%kernel.root_dir%/../path/to/public.pem' #By default is %kernel.root_dir%/../vendor/garlic/auth/src/Resources/jwt/public.pem

# Optional
JWT_PRIVATE_KEY_PATH=''
JWT_KEY_PASS_PHRASE='passPhrase'
JWT_TOKEN_TTL='3600'
JWT_USER_IDENTITY_FIELD='email'
JWT_USER_CLASS='Garlic\Auth\Security\User'
```

#### 3. Add Security configuration (change packages/security.yml):
```yaml
security:
    providers:
        jwt_provider:
            lexik_jwt:
                class: Garlic\Auth\Security\User
    firewalls:
        main:
            pattern:   ^/
            stateless: true
            anonymous: true
            guard:
                authenticators:
                    - lexik_jwt_authentication.jwt_token_authenticator
```

#### 4. Add Lexik jwt bundle configuration (change lexik_jwt_authentication.yml):
```yaml
lexik_jwt_authentication:
    private_key_path: '%env(JWT_PRIVATE_KEY_PATH)%'
    public_key_path:  '%env(JWT_PUBLIC_KEY_PATH)%'
    pass_phrase:      '%env(JWT_PASSPHRASE)%'
    token_ttl:        '%env(JWT_TOKEN_TTL)%'
    user_identity_field: email
    encoder:
        service: lexik_jwt_authentication.encoder.lcobucci
        crypto_engine:  openssl
        signature_algorithm: RS384
    token_extractors:
        authorization_header:
            enabled: true
            prefix:  ''
            name:    Authorization
```

### Using

#### 1. Make JWT token from the authorization service

#### 2. Add JWT token to to request headers:
Example:
```bash
Authorization: Body.Of.Token
```
