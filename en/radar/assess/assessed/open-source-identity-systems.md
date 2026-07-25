# open-source identity systems

directory services and identity and access management (IAM) platforms.

### LDAP: What it is

LDAP (Lightweight Directory Access Protocol) is a protocol for storing and querying directory information such as users, groups, and organizational data.

Common open-source LDAP servers include:

- OpenLDAP
- 389 Directory Server
- Apache Directory Server

LDAP provides:

- Centralized user directory
- Group management
- Authentication support for many applications
- Hierarchical data storage

LDAP does **not** natively provide modern features such as:

- Single Sign-On (SSO)
- OAuth2/OpenID Connect
- Social login
- Multi-factor authentication (MFA)
- User self-service portals

------

## Popular Open-Source Alternatives

### 1. Keycloak

Probably the most common modern alternative.

**Features**

- OAuth2
- OpenID Connect (OIDC)
- SAML
- Single Sign-On
- MFA
- Identity federation
- User self-service

**Pros**

- Modern web-based IAM
- Easy integration with cloud apps
- Active community
- Can connect to LDAP directories

**Cons**

- More complex than a simple LDAP server
- Higher resource requirements

**Best for**

- Modern applications
- Kubernetes environments
- SaaS authentication

------

### 2. FreeIPA

Combines multiple technologies:

- LDAP
- Kerberos
- DNS
- Certificate management

**Pros**

- Centralized Linux identity management
- Integrated Kerberos SSO
- Host and service management

**Cons**

- Linux-focused
- Less suited for web-app authentication than Keycloak

**Best for**

- Enterprise Linux environments
- Internal infrastructure

------

### 3. Authentik

A newer IAM platform.

**Features**

- OIDC
- SAML
- LDAP outpost support
- MFA
- User portal

**Pros**

- Modern UI
- Easier administration than some alternatives
- Active development

**Cons**

- Smaller ecosystem than Keycloak

**Best for**

- Self-hosters
- Small-to-medium organizations

------

### 4. Authelia

Often used with reverse proxies.

**Pros**

- Lightweight
- MFA
- Good for protecting internal services

**Cons**

- Not a full directory service
- Limited identity management capabilities

**Best for**

- Homelabs
- Internal dashboards and services

------

### 5. Kanidm

A newer project designed to modernize identity management.

**Pros**

- Modern architecture
- Built-in MFA
- Security-focused
- LDAP compatibility options

**Cons**

- Smaller ecosystem
- Less enterprise adoption

**Best for**

- Greenfield deployments
- Organizations prioritizing modern security

------

## Quick Comparison

| Feature            | LDAP (OpenLDAP) | Keycloak          | FreeIPA  | Authentik    | Kanidm     |
| ------------------ | --------------- | ----------------- | -------- | ------------ | ---------- |
| User Directory     | [X]             | [X]               | [X]      | [X]          | [X]        |
| LDAP Protocol      | [X]             | Limited/Federated | [X]      | LDAP support | Compatible |
| SSO                | No              | [X]               | [X]      | [X]          | [X]        |
| OAuth2/OIDC        | No              | [X]               | Limited  | [X]          | [X]        |
| MFA                | No              | [X]               | [X]      | [X]          | [X]        |
| Web UI             | Minimal         | Excellent         | Good     | Excellent    | Good       |
| Linux Host Mgmt    | No              | No                | [X]      | No           | No         |
| Modern App Support | Limited         | Excellent         | Moderate | Excellent    | Good       |

## Typical Recommendations

- **Need only a central directory?** → OpenLDAP or 389 Directory Server
- **Need SSO for web apps?** → Keycloak
- **Managing Linux servers and users?** → FreeIPA
- **Want a simpler modern IAM platform?** → Authentik
- **Starting fresh and want a modern architecture?** → Kanidm

If you describe your environment (Linux servers, Kubernetes, SaaS apps, internal applications, number of users, etc.), I can narrow this down to the most suitable option.

### If you're building modern applications

I'd strongly consider **Authentik** or **Keycloak** rather than starting with LDAP.

#### Why not LDAP first?

LDAP solves the directory problem, but most modern applications want:

- OpenID Connect (OIDC)
- OAuth2
- SSO
- MFA
- Self-service password resets
- Fine-grained application access control

With pure LDAP, you often end up adding an IAM layer later anyway.

------

### My ranking

#### 1. Authentik

I'd likely choose this for many new deployments.

Pros:

- Modern architecture and UI
- Easier administration than Keycloak for many teams
- OIDC, SAML, LDAP integration
- Good Kubernetes support
- Strong MFA and policy capabilities

Best when:

- Small-to-medium organizations
- Internal tools
- Self-hosted SaaS
- Platform engineering teams that don't need every enterprise IAM feature

------

#### 2. Keycloak

I'd choose this if I expected significant growth or enterprise requirements.

Pros:

- Largest ecosystem
- Widely adopted
- Extensive federation options
- Strong support across vendors and products

Cons:

- More operational complexity
- Steeper learning curve

Best when:

- Hundreds or thousands of users
- Multiple applications
- Enterprise integrations
- Future compliance requirements

------

#### 3. Kanidm

Technically very interesting.

Pros:

- Designed around modern security principles
- Simpler than traditional LDAP ecosystems
- Avoids some historical LDAP/Kerberos baggage

Cons:

- Smaller community
- Fewer integrations
- Less operational history

Best when:

- You like adopting newer infrastructure
- You control most of the applications

------

### When I'd still choose FreeIPA

If your primary goal is:

- Linux server logins
- SSH access management
- Centralized Unix identities
- Kerberos-based infrastructure

Then **FreeIPA** is still hard to beat.

Many organizations actually run:

- FreeIPA for infrastructure identities
- Keycloak or Authentik for application identities

------

### What I'd deploy today

For a typical greenfield startup or internal platform:

1. **Authentik**
2. OIDC everywhere possible
3. MFA enabled from day one
4. No LDAP unless a legacy application requires it

That gives you a simpler architecture and avoids building around older protocols when modern standards are available.