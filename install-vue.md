# installer vue

On fait référence à ce guide: https://fr.vuejs.org/v2/guide/installation.html

En prérequis, vous devez avoir installé nodejs sur votre ordinateur (https://nodejs.org/)

```bash
  # vous pouvez aussi installer globalement vue-cli
  npm install -g @vue/cli

  # on peut vérifier le numéro de version du cli
  npx @vue/cli -V
  # @vue/cli 5.0.1
  # si install global
  vue -V
  # @vue/cli 5.0.1
```

Voir la doc de vue-cli ici: https://cli.vuejs.org/

## Créer notre propre projet

```bash
# install non globale --> npx @vue/cli create my-project
vue create my-project
# remplacer my-project par le nom de votre projet
cd my-project
npm run serve

```
