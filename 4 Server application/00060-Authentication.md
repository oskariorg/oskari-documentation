## Authentication and user management (requires backend)

This section should give information about Spring Framework configurations and how applications can provide customized authentication and user management configurations.

Oskari has bundles for user management and for management of layers' priviledges. More information about the bundles will be added to this documentation later.

### Spring Security configurations

The Spring Security configurations are located at https://github.com/oskariorg/oskari-server/tree/develop/servlet-map/src/main/java/org/oskari/spring/security.

The configuration enabling an authentication based on Oskari's own database is defined in class `org.oskari.spring.security.database.OskariDatabaseSecurityConfig`. This class configures a `SecurityFilterChain` that uses the `OskariAuthenticationProvider`. Additionally, it sets up a form-based login and allows all requests (`permitAll`) while still handling user state.

`OskariAuthenticationProvider` implements Spring Security's `AuthenticationProvider` interface. It performs the actual database-based login and verification for users.

The configuration is active only when the `LoginDatabase` profile is enabled. This configuration can be replaced (for example, with a different SecurityFilterChain implementation) by defining a custom configuration under a new profile and setting the profile in `oskari-ext.properties` (`oskari.profiles=NewLoginConfiguration`).

The `DatabaseUserService`, presented in chapter [User Management](../5%20Functionalities/02000-UserManagement.md), can be used for user management even when a custom Spring Security configuration is in place. It is also possible to provide a custom user service implementation by creating a class that extends UserService.

### SSO authentication support

Oskari supports authentication and implementing/hooking into different authentication solutions.
