# Iniciando aplicativo mobile
## Configurando estrutura
Caso tenha instalado o `react-native-cli` globalmente, remover e iniciar o app pelo comando
```bash
npx react-native -init appgobarber --template react-native-template-typescript
```
Criar um `src/App.tsx`
```tsx
import React from 'react';
import {View} from 'react-native';

const App: React.FC = () => {
  return <View />;
};

export default App;
```

## Padrões de estilo
Remover os arquivos `App.tsx` e então fazer a importação desse `src/App.tsx` no `index.js` da raiz do projeto. Remover também `.eslintrc.js` e `.prettierrc.js`.

Para atualizar o eslint, fazemos a instalação novamente mas com a versão igual que o Diego está usando
```bash
yarn add -D eslint@^6.8.0
```

E iniciamos as configurações do eslint
```bash
yarn eslint --init
✔ How would you like to use ESLint? · style
✔ What type of modules does your project use? · esm
✔ Which framework does your project use? · react
✔ Does your project use TypeScript? · No / Yes
✔ Where does your code run? · browser
✔ How would you like to define a style for your project? · guide
✔ Which style guide do you want to follow? · airbnb
✔ What format do you want your config file to be in? · JSON
Checking peerDependencies of eslint-config-airbnb@latest
The config that youve selected requires the following dependencies:

eslint-plugin-react@^7.20.0 @typescript-eslint/eslint-plugin@latest eslint-config-airbnb@latest eslint@^5.16.0 || ^6.8.0 || ^7.2.0 eslint-plugin-import@^2.21.2 eslint-plugin-jsx-a11y@^6.3.0 eslint-plugin-react-hooks@^4 || ^3 || ^2.3.0 || ^1.7.0 @typescript-eslint/parser@latest
✔ Would you like to install them now with npm? · No / Yes
```

E depois fazemos a instalação com a versão igual do Diego
```bash
yarn add -D eslint-plugin-react@^7.20.0 @typescript-eslint/eslint-plugin@latest eslint-config-airbnb@latest eslint-plugin-import@^2.20.1 eslint-plugin-jsx-a11y@^6.2.3 eslint-plugin-react-hooks@^2.5.0 @typescript-eslint/parser@latest
```

Instalar o prettier
```bash
yarn add -D prettier eslint-config-prettier eslint-plugin-prettier
```

E por último,
```bash
yarn add -D eslint-import-resolver-typescript
```

Deletamos o eslintrc.json criado e copiamos os arquivos .editorconfig, .eslintignore, .eslintrc e prettier.config do projeto web. E removemos a opção do browser:
```json
{
  "env": {
    "es6": true
  },
  "extends": [
    "plugin:react/recommended",
    "airbnb",
    "plugin:@typescript-eslint/recommended",
    "prettier/@typescript-eslint",
    "plugin:prettier/recommended"
  ],
  "globals": {
    "Atomics": "readonly",
    "SharedArrayBuffer": "readonly"
  },
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaFeatures": {
      "jsx": true
    },
    "ecmaVersion": 2018,
    "sourceType": "module"
  },
  "plugins": [
    "react",
    "react-hooks",
    "@typescript-eslint",
    "prettier"
  ],
  "rules": {
    "prettier/prettier": "error",
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",
    "react/jsx-filename-extension": [1, { "extensions": [".tsx"] }],
    "react/jsx-props-no-spreading": "off",
    "react/prop-types": "off",
    "no-unused-expressions": "off",
    "@typescript-eslint/explicit-function-return-type": [
      "error",
      {
        "allowExpressions": true
      }
    ],
    "import/prefer-default-export": "off",
    "import/extensions": [
      "error",
      "ignorePackages",
      {
        "ts": "never",
        "tsx": "never"
      }
    ]
  },
  "settings": {
    "import/resolver": {
      "typescript": {}
    }
  }
}
```
E recarregamos o vscode para carregar o eslint.

## Configurando StatusBar
