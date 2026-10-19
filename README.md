# Organisation The Dot Side

Ce dépôt contient la [présentation publique](profile/README.md) et les conventions communes du collectif.

## Identité et accès

- Organisation GitHub : `thedotside` ; nom affiché : **The Dot Side**.
- Contact : `team.thedotside@gmail.com`.
- Chaque développeur utilise son compte personnel. L'adresse du collectif ne sert pas à créer un compte GitHub partagé.
- L'équipe `core` regroupe les mainteneurs ; les invitations nécessitent un identifiant fourni explicitement.
- Les droits se donnent dépôt par dépôt : `Maintain` pour l'entretien courant, `Admin` uniquement pour l'administration nécessaire.
- L'appartenance à `core` ne donne pas automatiquement des droits sur tous les futurs dépôts.

## Structure des dépôts

| Dépôt ou convention | Usage | Création |
| --- | --- | --- |
| `.github` | Présentation publique et conventions du collectif | Dépôt demandé pour cette mise en place |
| `<nom-du-projet>` | Un outil ou un ensemble de packages ayant le même cycle de vie | À créer lorsqu'un projet concret est demandé |
| `aspire-identityserver` | Candidat existant pour les intégrations Aspire IdentityServer | Un transfert depuis le compte personnel reste une opération distincte à décider |

Un même dépôt peut produire plusieurs packages. Un package ne justifie pas à lui seul un nouveau dépôt. Aucun dépôt vide n'est nécessaire pour réserver une idée.

Pour un projet .NET, privilégier `src/`, `tests/`, `samples/`, `eng/` et `.github/workflows/` lorsque ces dossiers ont un contenu utile. Pour un projet JavaScript ou TypeScript, ajouter `packages/` et `examples/` si le projet comporte plusieurs packages. Les artefacts de compilation et les secrets restent exclus du suivi Git.

## npm : `@thedotside`

L'organisation npm `thedotside` est créée et utilise le scope `@thedotside`. Son propriétaire initial est le compte personnel `gscbo`. Elle bénéficie de l'offre gratuite pour les packages publics. Chaque membre utilise son propre compte npm ; l'organisation npm reste indépendante de GitHub.

Convention : `@thedotside/<nom-du-package>`. Exemple de métadonnées à adapter dans un futur projet réel :

```json
{
  "name": "@thedotside/exemple",
  "publishConfig": {
    "access": "public"
  }
}
```

Le nom `exemple` est illustratif : ce guide ne crée ni ne publie ce package. Avant toute publication, vérifier le contenu avec `npm pack --dry-run`, les tests, la licence et le champ `repository`. Les packages publics sous scope se publient avec `npm publish --access public`.

Activer la 2FA des comptes de publication. Pour une CI, privilégier le trusted publishing OIDC limité au dépôt et au workflow réels. Ne créer cette relation de confiance qu'au moment de préparer une vraie publication ; elle n'est pas configurée par ce document.

Sources : [créer une organisation](https://docs.npmjs.com/creating-an-organization/), [publier sous le scope d'une organisation](https://docs.npmjs.com/creating-and-publishing-an-organization-scoped-package/), [trusted publishing](https://docs.npmjs.com/trusted-publishers/).

## NuGet : `TheDotSide.*`

Préparer une organisation NuGet depuis un compte individuel, avec le nom souhaité `TheDotSide` et l'adresse du collectif. Le nom et l'adresse doivent être uniques sur NuGet.org ; leur disponibilité doit être validée dans l'interface.

Convention proposée pour les futurs packages : `TheDotSide.<Projet>.<Composant>`.

Pour Aspire IdentityServer, la cible de nommage est :

| Package actuel | Package cible proposé |
| --- | --- |
| `Aspire.IdentityServer.Hosting` | `TheDotSide.Aspire.IdentityServer.Hosting` |
| `Aspire.IdentityServer.Runtime` | `TheDotSide.Aspire.IdentityServer.Runtime` |
| `Aspire.IdentityServer.Client` | `TheDotSide.Aspire.IdentityServer.Client` |

Ce tableau prépare la migration ; il ne renomme pas les packages existants et n'atteste pas leur disponibilité. La migration devra aligner les `PackageId`, les dépendances entre packages, les scripts de packaging, les tests consommateurs et la documentation. Les namespaces et les noms d'assemblies peuvent rester distincts des identifiants NuGet.

Renseigner `Authors` avec `The Dot Side`, les métadonnées de licence, le README et l'URL réelle du dépôt. Conserver Duende et les autres composants tiers comme dépendances NuGet ordinaires, avec leurs licences propres. Cette intégration IdentityServer reste destinée au développement et aux tests locaux.

Avant toute publication d'Aspire IdentityServer, exécuter les validations du projet : `dotnet build`, `dotnet test`, `pwsh eng/pack.ps1`, puis `pwsh eng/package-smoke.ps1`. Une compilation locale ne prouve ni une publication ni la réussite d'une CI distante.

Pour la CI de publication, préparer une politique NuGet Trusted Publishing OIDC limitée au propriétaire, au dépôt et au fichier de workflow réels. Choisir l'organisation comme propriétaire de la politique et cibler les packages voulus. Ne pas enregistrer de clé ou de jeton dans Git.

### Réservation du préfixe

Le motif souhaité est **`TheDotSide.*`**. La création de l'organisation, les métadonnées et la publication d'un premier package ne réservent pas automatiquement ce préfixe.

La procédure officielle consiste à contacter `account@nuget.org` avec le nom affiché du propriétaire NuGet et le motif demandé. Fournir les preuves d'identité du collectif et ses liens publics. L'équipe NuGet examine notamment le lien clair entre préfixe et propriétaire, son caractère non générique et le risque de confusion pour la communauté. Elle peut demander des informations complémentaires, accepter ou refuser.

L'attribution n'est acquise qu'après acceptation explicite de NuGet. Une recherche sans résultat n'est pas une preuve de disponibilité. Les packages préexistants appartenant à des tiers ne sont pas retirés par une réservation ultérieure.

Sources : [organisations NuGet](https://learn.microsoft.com/en-us/nuget/nuget-org/organizations-on-nuget-org), [réservation des préfixes](https://learn.microsoft.com/en-us/nuget/nuget-org/id-prefix-reservation), [trusted publishing NuGet](https://learn.microsoft.com/en-us/nuget/nuget-org/trusted-publishing).
