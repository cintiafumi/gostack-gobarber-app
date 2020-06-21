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
Importar StatusBar em App.tsx
```tsx
import React from 'react';
import { View, StatusBar } from 'react-native';

const App: React.FC = () => (
  <>
    <StatusBar barStyle="light-content" backgroundColor="#312e38" />
    <View style={{ flex: 1, backgroundColor: '#312e38' }} />
  </>
);

export default App;
```

## Configurando navegação
Vamos criar as páginas de SignIn e SignUp.

Importar o styled-components
```bash
yarn add styled-components
yarn add -D @types/styled-components
```

Criamos o arquivo de rotas e instalamos o pacote conforme [doc](https://reactnavigation.org/docs/getting-started)
```bash
yarn add @react-navigation/native

yarn add react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view

cd ios && pod install && cd ..
```
Para o android não precisamos instalar nada.

Ir para o `App.tsx` e importar acima de tudo
```tsx
import 'react-native-gesture-handler';
```

Toda vez que mexemos em código nativo (pod install), temos que executar de novo
```bash
yarn ios
```

Em `src/pages/SignIn/index.tsx`
```tsx
import React from 'react';

import { Container } from './styles';

const SignIn: React.FC = () => {
  return <Container />;
};

export default SignIn;
```

Em `src/pages/SignIn/styles.ts`
```ts
import styled from 'styled-components/native';

export const Container = styled.View``;
```

Em SignUp, fazemos o mesmo.

Vamos instalar também conforme [doc](https://reactnavigation.org/docs/hello-react-navigation)
```bash
yarn add @react-navigation/stack
```

Para cada navegação, vamos criando uma navegação diferente. Em `src/routes/index.tsx`
```tsx
import React from 'react';
import { createStackNavigator } from '@react-navigation/stack';

import SignIn from '../pages/SignIn';
import SignUp from '../pages/SignUp';

const Auth = createStackNavigator();

const AuthRoutes: React.FC = () => (
  <Auth.Navigator
    screenOptions={{
      headerShown: false,
      cardStyle: { backgroundColor: '#312e38' },
    }}
  >
    <Auth.Screen name="SignIn" component={SignIn} />
    <Auth.Screen name="SignUp" component={SignUp} />
  </Auth.Navigator>
);

export default AuthRoutes;
```

Por volta da minha aplicação inteira preciso ter o `NavigationContainer`
```tsx
import 'react-native-gesture-handler';

import React from 'react';
import { View, StatusBar } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';

import Routes from './routes';

const App: React.FC = () => (
  <NavigationContainer>
    <StatusBar barStyle="light-content" backgroundColor="#312e38" />
    <View style={{ flex: 1, backgroundColor: '#312e38' }}>
      <Routes />
    </View>
  </NavigationContainer>
);

export default App;
```

## Densidade de pixel
Quando exportamos uma imagem do Figma para usar no app mobile, temos que exportar 1x, 2x e 3x, pois cada dispositivo tem uma densidade diferente de pixel na tela. Vamos colocar as imagens em `src/assets`

Para corrigir a importação do png, vamos criar um arquivo `src/@types/index.d.ts`
```ts
declare module '*.png';
```

Em `src/pages/SignIn/index.tsx`
```tsx
import React from 'react';
import { Image } from 'react-native';

import { Container } from './styles';

import logoImg from '../../assets/logo.png';

const SignIn: React.FC = () => {
  return (
    <Container>
      <Image source={logoImg} />
    </Container>
  );
};

export default SignIn;
```

## Importando fontes externas
Procurar a fonte no google fonts e baixar. Criamos a pasta na raiz do projeto `assets/fonts` e colocamos as fontes dentro dessa pasta. Detalhe para android é que o nome da fonte será exatamente o nome do arquivo. No caso do ios, existe um nome escondido, então vamos instalar essas fontes no sistema e procuramos no macbook por font book. Procuramos por roboto slab e ao clicar no ícone de info, vemos que o nome que procuramos é `PostScript name`. A sugestão é que o nome do arquivo da fonte seja o mesmo que desse `PostScript name` para funcionar para ambos dispositivos.

Criamos agora um arquivo na raiz do projeto chamado `react-native.config.js`
```js
module.exports = {
  project: {
    ios: {},
    android: {},
  },
  assets: [
    './assets/fonts'
  ]
}
```

E no terminal rodamos
```bash
yarn react-native link
```
E vemos que as fontes foram inseridas em `android/app/src/main/assets/fonts` e em `ios/appgobarber/Info.plist`. Como rodamos algo nativo, precisamos rodar novamente `yarn ios` ou `yarn android`.

Para usarmos as fontes, vamos criar um componente em SignIn
```tsx
import { Container, Title } from './styles';
//...
      <Title>Faça seu logon</Title>
```

E em `src/pages/SignIn/styles.ts`
```ts
import styled from 'styled-components/native';

export const Container = styled.View`
  flex: 1;
  align-items: center;
  justify-content: center;
`;

export const Title = styled.Text`
  font-size: 24px;
  color: #f4ede8;
  font-family: 'RobotoSlab-Medium';
  margin: 64px 0 24px;
`;
```

Uma coisa que não funciona é o encadeamento tipo Sass.
