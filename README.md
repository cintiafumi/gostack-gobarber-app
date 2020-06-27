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

# Autenticação e cadastro
## Input & Button
Criar a pasta `components` com Input e Button. Importar os componentes no SignIn.

Em Button, recebemos `children` que será um `<Text>`. Para a estilização de Button, usamos `RectButton` da biblioteca `react-native-gesture-handler` que tem o efeito de click para ambas plataformas e sua `RectButtonProperties` para `Props` deixando `children` obrigatório e sendo `string`.

Em Input, usamos o `TextInput` do próprio `react-native` para o styled e o `TextInputProps` para passar suas Props. No input, vamos deixar `name` e `icon` com propriedades obrigatórias. E adicionamos essas informações em `SignIn`. Pegamos todas as props e repassamos para o componente `TextInput`. A propriedade `keyboardAppearance` somente funciona para iOS e deixa a cor do teclado escura.

Para os ícones, instalamos o pacote `react-native-vector-icons` e damos novamente `pod install` dentro da pasta ios. E em `ios/appgobarber/Info.plist` e adicionamos
```
		<string>Feather.ttf</string>
```

Dentro do arquivo `android/app/build.gradle` e adicionar no final
```
project.ext.vectoricons = [
  iconFontNames: ['Feather.ttf']
];

apply from: "../../node_modules/react-native-vector-icons/fonts.gradle"
```

E adicionamos a tipagem do pacote
```bash
yarn add -D @types/react-native-vector-icons
```

Importamos o componente `FeatherIcon` que terá todos os icons a partir dele.

Em `src/components/Input/index.tsx`
```tsx
import React from 'react';
import { TextInputProps } from 'react-native';

import { Container, TextInput, Icon } from './styles';

interface InputProps extends TextInputProps {
  name: string;
  icon: string;
}

const Input: React.FC<InputProps> = ({ name, icon, ...rest }) => (
  <Container>
    <Icon name={icon} size={20} color="#666360" />

    <TextInput
      keyboardAppearance="dark"
      placeholderTextColor="#666360"
      {...rest}
    />
  </Container>
);

export default Input;
```

Em `src/components/Input/styles.ts`
```ts
import styled from 'styled-components/native';
import FeatherIcon from 'react-native-vector-icons/Feather';

export const Container = styled.View`
  width: 100%;
  height: 60px;
  padding: 0 16px;
  background: #232129;
  border-radius: 10px;
  margin-bottom: 8px;
  flex-direction: row;
  align-items: center;
`;

export const TextInput = styled.TextInput`
  flex: 1;
  color: #fff;
  font-size: 16px;
  font-family: 'RobotoSlab-Regular';
`;

export const Icon = styled(FeatherIcon)`
  margin-right: 16px;
`;
```

Em `src/components/Button/styles.ts`
```ts
import styled from 'styled-components/native';
import { RectButton } from 'react-native-gesture-handler';

export const Container = styled(RectButton)`
  width: 100%;
  height: 50px;
  background: #ff9000;
  border-radius: 10px;
  margin-top: 8px;

  justify-content: center;
  align-items: center;
`;

export const ButtonText = styled.Text`
  font-family: 'RobotoSlab-Medium';
  color: #312e38;
  font-size: 18px;
`;
```

Em `src/components/Button/index.tsx`
```tsx
import React from 'react';
import { RectButtonProperties } from 'react-native-gesture-handler';
import { Container, ButtonText } from './styles';

interface ButtonProps extends RectButtonProperties {
  children: string;
}

const Button: React.FC<ButtonProps> = ({ children, ...rest }) => (
  <Container {...rest}>
    <ButtonText>{children}</ButtonText>
  </Container>
);

export default Button;
```

Em `src/components/Button/styles.ts`
```ts
import styled from 'styled-components/native';
import { RectButton } from 'react-native-gesture-handler';

export const Container = styled(RectButton)`
  width: 100%;
  height: 50px;
  background: #ff9000;
  border-radius: 10px;
  margin-top: 8px;

  justify-content: center;
  align-items: center;
`;

export const ButtonText = styled.Text`
  font-family: 'RobotoSlab-Medium';
  color: #312e38;
  font-size: 18px;
`;
```

Em `src/pages/SignIn/index.tsx`
```tsx
      <Input name="email" icon="mail" placeholder="E-mail" />

      <Input name="password" icon="lock" placeholder="Senha" />

      <Button
        onPress={() => {
          console.log('Deu');
        }}
      >
```

## Tela de autenticação
Vamos adicionar os links para "Esqueci minha senha" na tela de `SignIn`.
```tsx
        <ForgotPassword onPress={() => {}}>
          <ForgotPasswordText>Esqueci minha senha</ForgotPasswordText>
        </ForgotPassword>
```

Com o estilo
```ts
export const ForgotPassword = styled.TouchableOpacity`
  margin-top: 24px;
`;

export const ForgotPasswordText = styled.Text`
  color: #f4ede8;
  font-size: 16px;
  font-family: 'RobotoSlab-Regular';
`;
```

E também o link de "Criar uma conta" que como ficará com `position: absolute;`, terá que ficar fora do container.
```tsx
      <CreateAccountButton>
        <Icon name="log-in" size={20} color="#ff9000" />
        <CreateAccountButtonText>Criar uma conta</CreateAccountButtonText>
      </CreateAccountButton>
```
Para seu estilo, importamos um pacote que ajuda a vermos a distância inferior do tracinho
```bash
yarn add react-native-iphone-x-helper
```

```ts
export const CreateAccountButton = styled.TouchableOpacity`
  position: absolute;
  left: 0;
  bottom: 0;
  right: 0;
  background: #312e38;
  border-top-width: 1px;
  border-color: #232129;
  padding: 16px 0 ${16 + getBottomSpace()}px;

  justify-content: center;
  align-items: center;
  flex-direction: row;
`;

export const CreateAccountButtonText = styled.Text`
  color: #ff9000;
  font-size: 18px;
  font-family: 'RobotoSlab-Regular';
  margin-left: 16px;
`;
```

Agora, quando clicamos no input e o teclado sobe, precisamos arrumar a posição para o input ficar visível
```tsx
import {
  Image,
  View,
  ScrollView,
  KeyboardAvoidingView,
  Platform,
} from 'react-native';

const SignIn: React.FC = () => {
  return (
    <>
      <KeyboardAvoidingView
        style={{ flex: 1 }}
        behavior={Platform.OS === 'ios' ? 'padding' : undefined}
      >
        <ScrollView
          contentContainerStyle={{ flex: 1 }}
          keyboardShouldPersistTaps="handled"
        >
          <Container>
```
Onde o `KeyboardAvoidingView` vai empurrar o conteúdo para cima, o `ScrollView` vai permitir o scroll da tela e também vai fazer o teclado fechar quando tocar na tela fora do input.

## Tela de cadastro
Copia tudo do SignIn para o SignUp
```tsx
import React from 'react';
import {
  Image,
  View,
  ScrollView,
  KeyboardAvoidingView,
  Platform,
} from 'react-native';
import Icon from 'react-native-vector-icons/Feather';

import Input from '../../components/Input';
import Button from '../../components/Button';

import logoImg from '../../assets/logo.png';

import { Container, Title, BackToSignIn, BackToSignInText } from './styles';

const SignUp: React.FC = () => {
  return (
    <>
      <KeyboardAvoidingView
        style={{ flex: 1 }}
        behavior={Platform.OS === 'ios' ? 'padding' : undefined}
      >
        <ScrollView
          contentContainerStyle={{ flex: 1 }}
          keyboardShouldPersistTaps="handled"
        >
          <Container>
            <Image source={logoImg} />

            <View>
              <Title>Crie sua conta</Title>
            </View>

            <Input name="name" icon="user" placeholder="Nome" />

            <Input name="email" icon="mail" placeholder="E-mail" />

            <Input name="password" icon="lock" placeholder="Senha" />

            <Button
              onPress={() => {
                console.log('Deu');
              }}
            >
              Entrar
            </Button>

            <BackToSignIn onPress={() => {}}>
              <Icon name="arrow-left" size={20} color="#fff" />
              <BackToSignInText>Voltar para logon</BackToSignInText>
            </BackToSignIn>
          </Container>
        </ScrollView>
      </KeyboardAvoidingView>
    </>
  );
};

export default SignUp;
```

Com estilo
```ts
import styled from 'styled-components/native';
import { getBottomSpace } from 'react-native-iphone-x-helper';
import { Platform } from 'react-native';

export const Container = styled.View`
  flex: 1;
  align-items: center;
  justify-content: center;
  padding: 0 30px ${Platform.OS === 'android' ? 150 : 100}px;
`;

export const Title = styled.Text`
  font-size: 24px;
  color: #f4ede8;
  font-family: 'RobotoSlab-Medium';
  margin: 64px 0 24px;
`;

export const BackToSignIn = styled.TouchableOpacity`
  position: absolute;
  left: 0;
  bottom: 0;
  right: 0;
  background: #312e38;
  border-top-width: 1px;
  border-color: #232129;
  padding: 16px 0 ${16 + getBottomSpace()}px;

  justify-content: center;
  align-items: center;
  flex-direction: row;
`;

export const BackToSignInText = styled.Text`
  color: #fff;
  font-size: 18px;
  font-family: 'RobotoSlab-Regular';
  margin-left: 16px;
`;
```

E para linkar as telas, utilizamos o `useNavigation` em SignUp
```tsx
import { useNavigation } from '@react-navigation/native';

            <BackToSignIn onPress={() => navigation.goBack()}>
```

E em SignIn
```tsx
      <CreateAccountButton onPress={() => navigation.navigate('SignUp')}>
```

