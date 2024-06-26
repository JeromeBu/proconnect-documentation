# Quelles sont les données que je peux récupérer par AgentConnect sur les agents ?

Les données Agent sont fournies par les Fournisseurs d'Identité aux Fournisseurs de Services, via AgentConnect, conformément à l'habilitation obtenue via [datapass.api.gouv.fr](https://datapass.api.gouv.fr), et le choix des données réalisé par le Fournisseur de Services dans cette demande.


## Les données obligatoires

En plus de l'openid, qui est obligatoire, des données obligatoires sont fournies par les Fournisseurs d'Identité aux Fournisseurs de Services via AgentConnect. Ces données permettent d'identifier un utilisateur.
                                                    

|Champs | Obligatoire | Description| Format |
|---- | ------ | ------ | ------ |
|given_name | Oui |Prénoms séparés par des espaces (standard OpenIDConnect)| UTF-8 (standard OpenIDConnect)|
|usual_name| Oui |Nom de famille d'usage (par défaut = family_name)| UTF-8 |
|email | Oui |Adresse courriel |UTF-8 (standard OpenIDConnect)|
|uid|Oui |Identifiant unique de l'agent auprès du FI| String (standard OpenIDConnect)|
| siret | Oui |Identifiant d'établissement| string, 14 chiffres sans espace|

En complément, il est possible d'obtenir des données complémentaires. Cependant ces données ne sont pas obligatoirement connues par tous les Fournisseurs d'Identité.


## Les données complémentaires

Champs | Obligatoire | Description| Format |
|---- | ------ | ------ | ------ |
| siren | non  | Identifiant d'entreprise  | String, 9 chiffres sans espace |
| organizational_unit  | non  | Ministère/Direction/Service d'affectation   | UTF8 |
| belonging_population  | non  | Population d'appartenance  | string, Exemple: agent, prestataire, partenaire, stagiaire |
| phone  | non  | Téléphones de contact  | Format non normé |
| chorusdt   | Non | Entité ministérielle/Matricule Agent  | string |

La liste des données complémentaires est non exhaustive et pourra être amendée si besoin.

AgentConnect transmet systématiquement au Fournisseur de Services un identifiant unique pour chaque agent (le sub) : 

* Cet identifiant (appelé sub) est spécifique à chaque Fournisseur de Services et à chaque Fournisseur d'Identité: un même utilisateur aura toujours le même SUB pour un Fournisseur d'Identité donné, en revanche il aura un SUB différent pour chaque Fournisseur de Services qu'il utilisera.

:warning: **Attention**

**Réconciliation**
> Il ne faut pas effectuer une réconciliation sur le SUB, car le SUB est calculé en fonction du FI utilisé. Nous vous recommandons l'utilisation de l'email professionnel pour cet usage.

## La liste des scopes disponibles lors de l'étape d'authentification AgentConnect

AgentConnect a étendu le mécanisme de scopes pour qu'il soit plus modulaire.

* Un seul scope est obligatoire : openid. Il permet de récupérer le sub (identifiant unique technique) de l'utilisateur.
* Il est possible de combiner plusieurs scopes de son choix pour récupérer seulement les informations dont a besoin le FS.


Cette liste de scopes est définie par la norme OpenIDConnect : http://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims
