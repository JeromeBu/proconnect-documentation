# Configurer OpenID Connect (OIDC) pour AgentConnect en tant que Fournisseur d'Identité (FI)

## Test de la connexion à votre FI
Vous pouvez à tout moment tester l'intégration de votre FI à la fédération AgentConnect. Vous trouverez les informations de test [ici](./test-configuration-fi.md).
En cas d'erreur, deux documents vous permettront d'analyser vos erreurs :
- [le troubleshooting](./troubleshooting.md)
- [la liste des codes d'erreurs possibles renvoyés par AgentConnect](https://github.com/france-connect/sources/blob/main/back/_doc/erreurs.md)

## Trouver la Discovery URL
La Discovery URL est une URL **publique** fournie par le FI qui expose un ensemble d'informations nécessaires à la bonne interaction avec les clients OIDC. Elle se termine nécessairement par `/.well-known/openid-configuration`.

À titre d'exemple, la Discovery URL de l'INSEE est la suivante : https://auth.insee.net/auth/realms/insee/.well-known/openid-configuration

Cette URL doit être conservée pour être envoyée à AgentConnect par le FI une fois la configuration faite.

## Créer un client
Commencez par créer un client OIDC pour AgentConnect. Vous pouvez choisir comme `client_id` "agentconnect" par exemple. Pour le `client_secret`, vous pouvez le générer de votre côté ou à l'aide d'outils en ligne comme [randomgenerate.io](https://randomgenerate.io/random-string-generator).
Ces deux valeurs doivent être conservées de votre côté pour être envoyées à AgentConnect une fois la configuration faite.

La `redirect_uri` (ou "adresse de redirection de connexion") à indiquer est la suivante :
https://AC_DOMAIN/api/v2/oidc-callback 

Vous pouvez retrouver la valeur de AC_DOMAIN qui vous correspond [ici](../resources/valeur_ac_domain.md)

## Configurer la signature des échanges entre AC et le FI
Les appels aux endpoints de création de jeton (`/token`) et de récupération des informations utilisateur (`/user-info`) par AgentConnect doivent être signés.
AgentConnect gère trois algorithmes de signatures :
- Asymétrique : 

       - ES256 (EC + SHA256)
       - RS256 (RSA + SHA256)

- Symétrique : géré mais non recommandé

       - HS256 (HMAC + SHA256) 

Si vous sélectionnez un algorithme de signature asymétrique, les clefs de signatures doivent être disponible via la *JWKS URL* présente dans les méta-data de la *Discovery URL*. 


Les spécifications des algorithmes de signatures utilisés sont les suivants : 
* [JWA - https://tools.ietf.org/html/rfc7518](https://tools.ietf.org/html/rfc7518)
* [JWS - https://tools.ietf.org/html/rfc7515#appendix-A.3](https://tools.ietf.org/html/rfc7515#appendix-A.3)
* [JWE - https://tools.ietf.org/html/rfc7516#appendix-A.1](https://tools.ietf.org/html/rfc7516#appendix-A.1)

## Configurer les scopes
La liste des scopes demandés par AgentConnect est la suivante :
| Scopes       |
| --- | 
| openid | 
| given_name |
| usual_name| 
| email |
| uid | 
| siren | 
| siret | 
| organizational_unit | 
| belonging_population | 
| phone |
| chorusdt |

Parmi ces champs, seuls sont obligatoires :
|Champs  | Description| Format |
|----  | ------ | ------ |
|given_name  |Prénoms séparés par des espaces (standard OpenIDConnect)| UTF-8 (standard OpenIDConnect)|
|usual_name |Nom de famille d'usage (par défaut = family_name)| UTF-8 |
|email  |Adresse courriel |UTF-8 (standard OpenIDConnect)|
|uid |Identifiant unique de l'agent auprès du FI| String (standard OpenIDConnect)|

Les autres champs, si vous ne possédez pas l'information correspondante dans votre annuaire, doivent être **ignorés**. (cf. [RFC OIDC](https://openid.net/specs/openid-connect-core-1_0.html#IDToken) : *Any Claims used that are not understood MUST be ignored.*)
Selon votre Fournisseur d'Identité, il est possible qu'il vous faille spécifier, pour chacun des scopes demandés, une valeur de retour nulle, indéfinie, ou simplement ignorée.
