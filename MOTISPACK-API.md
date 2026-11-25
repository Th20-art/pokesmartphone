# Contrôler un mod en jeu depuis MotisPack

## Fiche et réglages d’un mod communautaire

Le fichier [communaute.json](communaute.json) est récupéré par le bouton **Actualiser** dans MotisPack. Utilise l’identifiant Fabric du mod dans `id`. Le champ facultatif `commande_reglages` contient le nom d’une commande locale telle que `pokehud`, sans barre oblique ni arguments. MotisPack affiche son nom et l’exécute sur le client au clic si le mod et la commande sont disponibles.

## Bouton d’activation en jeu

Dans le `fabric.mod.json` du mod client, indique la classe de réglages.

```json
{
  "custom": {
    "motispack:settings": "exemple.client.SettingsApi"
  }
}
```

Cette classe publique expose deux méthodes statiques.

```java
public static boolean isEnabled()
public static void setEnabled(boolean enabled)
```

MotisPack y accède depuis le thread client. L’état retourné correspond à l’effet en jeu. Lors de la désactivation, arrête les traitements, restaure les états du jeu modifiés par le mod et ferme ses activités en cours. Les callbacks et mixins déjà enregistrés restent en place. Ils vérifient cet état pour rendre la main au jeu. Lors de la réactivation, reprends le traitement sans enregistrer les callbacks une seconde fois.

Les deux méthodes suffisent pour que le bouton principal de MotisPack bascule le mod dans la session en cours. Le mod peut enregistrer l’état dans sa propre configuration. Pour un mod qui conserve des réglages détaillés, les méthodes `switches()`, `get(String)` et `set(String, boolean)` restent disponibles.

Pour tester le contrat, effectue deux cycles de désactivation et de réactivation dans la même connexion. Vérifie les touches, le rendu et les actions en attente pendant chaque transition.
