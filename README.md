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

## Integrando Unform
Instalação
```bash
yarn add @unform/core @unform/mobile
```

Colocar o `Form` em volta dos inputs e button de SignIn, lembrando que precisamos colocar a referência do form no click do button
```tsx
import { Form } from '@unform/mobile';
import { FormHandles } from '@unform/core';

const SignIn: React.FC = () => {
  const formRef = useRef<FormHandles>(null);
  const handleSignIn = useCallback((data: object) => {
    console.log(data);
  }, []);
//...
            <Form ref={formRef} onSubmit={handleSignIn}>
              <Input name="email" icon="mail" placeholder="E-mail" />

              <Input name="password" icon="lock" placeholder="Senha" />

              <Button
                onPress={() => {
                  formRef.current?.submitForm();
                }}
              >
                Entrar
              </Button>
            </Form>
```

E no Input também vamos alterar para então receber o valor digitado, que é diferente que na web
```tsx
import React, { useRef, useEffect } from 'react';
import { TextInputProps } from 'react-native';
import { useField } from '@unform/core';

import { Container, TextInput, Icon } from './styles';

interface InputProps extends TextInputProps {
  name: string;
  icon: string;
}

interface InputValueReference {
  value: string;
}

const Input: React.FC<InputProps> = ({ name, icon, ...rest }) => {
  const inputElementRef = useRef<any>(null);

  const { registerField, defaultValue = '', fieldName, error } = useField(name);
  const inputValueRef = useRef<InputValueReference>({ value: defaultValue });

  useEffect(() => {
    registerField<string>({
      name: fieldName,
      ref: inputValueRef.current,
      path: 'value',
      setValue(ref: any, value) {
        inputValueRef.current.value = value;
        inputElementRef.current.setNativeProps({ text: value });
      },
      clearValue() {
        inputValueRef.current.value = '';
        inputElementRef.current.clear();
      },
    });
  }, [fieldName, registerField]);

  return (
    <Container>
      <Icon name={icon} size={20} color="#666360" />

      <TextInput
        keyboardAppearance="dark"
        placeholderTextColor="#666360"
        defaultValue={defaultValue}
        onChangeText={(value) => {
          inputValueRef.current.value = value;
        }}
        {...rest}
      />
    </Container>
  );
};

export default Input;
```

E no `Form`, adicionamos
```tsx
import { Form } from '@unform/mobile';
import { FormHandles } from '@unform/core';
//...
  const formRef = useRef<FormHandles>(null);

  const handleSignIn = useCallback((data: object) => {
    console.log(data);
  }, []);
//...
            <Form ref={formRef} onSubmit={handleSignIn}>
              <Input name="email" icon="mail" placeholder="E-mail" />

              <Input name="password" icon="lock" placeholder="Senha" />

              <Button
                onPress={() => {
                  formRef.current?.submitForm();
                }}
              >
                Entrar
              </Button>
            </Form>
```

## Usabilidade em formulários
Desabilitar a correção automática nos inputs em SignIn
```tsx
//...
  const passwordInputRef = useRef<TextInput>(null);
//...
              <Input
                autoCorrect={false}
                autoCapitalize="none"
                keyboardType="email-address"
                name="email"
                icon="mail"
                placeholder="E-mail"
                returnKeyType="next"
              />

              <Input
                ref={passwordInputRef}
                name="password"
                icon="lock"
                placeholder="Senha"
                secureTextEntry
                returnKeyType="send"
                onSubmitEditing={() => {
                  formRef.current?.submitForm();
                }}
              />
```

Mas para conseguirmos passar o ref para o Input, temos que fazer essa alteração no Input. O componente não será mais `React.FC` mas sim `React.RefForwardingComponent`. O hook `useImperativeHandle` serve para passar do componente filho para componente pai, onde o primeiro parâmetro é o `ref` e o segundo é uma função que vai retornar o que quero para o componente pai. E temos que englobar o no `export default` com `forwardRef`
```tsx
import React, {
  useRef,
  useEffect,
  useImperativeHandle,
  forwardRef,
} from 'react';

interface InputRef {
  focus(): void;
}

const Input: React.RefForwardingComponent<InputRef, InputProps> = (
  { name, icon, ...rest },
  ref,
) => {
  useImperativeHandle(ref, () => ({
    focus() {
      inputElementRef.current.focus();
    },
  }));
//...
export default forwardRef(Input);
```

## Foco e preenchimento
Adicionar em Input
```tsx
  const [isFocused, setIsFocused] = useState(false);
  const [isFilled, setIsFilled] = useState(false);

  const handleInputFocus = useCallback(() => {
    setIsFocused(true);
  }, []);

  const handleInputBlur = useCallback(() => {
    setIsFocused(false);

    setIsFilled(!!inputValueRef.current.value);
  }, []);
//..
      <Icon
        //...
        color={isFocused || isFilled ? '#ff9000' : '#666360'}
      />

      <TextInput
        //...
        onFocus={handleInputFocus}
        onBlur={handleInputBlur}
        //...
      />
```

Adicionar nos style do Input
```ts
interface ContainerProps {
  isFocused: boolean;
}

export const Container = styled.View<ContainerProps>`

  border-width: 2px;
  border-color: #232129;

  ${(props) =>
    props.isFocused &&
    css`
      border-color: #ff9000;
    `}
`;
```

## Validação dos formulários
Adicionamos o yup
```bash
yarn add yup
yarn add -D @types/yup
```

Vamos copiar do projeto web a pasta `utils/getValidationError.ts` e toda a função `handleSubmit` de `pages/SignIn` do projeto web. E fazemos algumas modificações
```tsx
  const handleSignIn = useCallback(async (data: SignInFormData) => {
    try {
      formRef.current?.setErrors({});

      const schema = Yup.object().shape({
        email: Yup.string()
          .required('E-mail obrigatório')
          .email('Digite um e-mail válido'),
        password: Yup.string().required('Senha obrigatória'),
      });

      await schema.validate(data, {
        abortEarly: false,
      });

      // await signIn({
      //   email: data.email,
      //   password: data.password,
      // });

      // history.push('/dashboard');
    } catch (err) {
      if (err instanceof Yup.ValidationError) {
        const errors = getValidationErrors(err);
        formRef.current?.setErrors(errors);

        return;
      }

      Alert.alert(
        'Erro de autenticação',
        'Ocorreu um erro ao fazer login. Verifique as credenciais.',
      );
    }
  }, []);
  ```
No Input também precisamos receber agora o erro
```tsx
    <Container isFocused={isFocused} hasError={!!error}>
```
E no seu estilo, adicionamos a borda vermelha antes da borda do focus
```ts
interface ContainerProps {
  isFocused: boolean;
  hasError: boolean;
}
//...
  ${(props) =>
    props.hasError &&
    css`
      border-color: #c53030;
    `}
```

Agora copiamos do SignUp do projeto web
```tsx
  const handleSignUp = useCallback(async (data: SignUpFormData) => {
    try {
      formRef.current?.setErrors({});

      const schema = Yup.object().shape({
        name: Yup.string().required('Nome obrigatório'),
        email: Yup.string()
          .required('E-mail obrigatório')
          .email('Digite um e-mail válido'),
        password: Yup.string().min(6, 'No mínimo 6 caracteres'),
      });

      await schema.validate(data, {
        abortEarly: false,
      });

      // await api.post('/users', data);

      // history.push('/');
    } catch (err) {
      if (err instanceof Yup.ValidationError) {
        const errors = getValidationErrors(err);
        formRef.current?.setErrors(errors);

        return;
      }

      Alert.alert(
        'Erro no cadastro',
        'Ocorreu um erro ao fazer cadastro. Tente novamente.',
      );
    }
  }, []);
  ```

# Conectando com a API
## Cadastro na aplicação
Adicionamos o axios
```bash
yarn add axios
```
Deixamos o backend e o docker da aplicação rodando.

Adicionamos o `services/api.ts` como sempre
```ts
import axios from 'axios';

const api = axios.create({
  baseURL: 'http://localhost:3333',
});

export default api;
```

E em SignUp vamos adicionar então
```tsx
import api from '../../services/api';

      await api.post('/users', data);

      Alert.alert(
        'Cadastro realizado com sucesso!',
        'Você já pode fazer login na aplicação.',
      );

      navigation.goBack();
```

## Auttenticação de usuário
Vamos copiar `src/hooks/auth.tsx` da aplicação web. O que muda agora é ao invés de localStorage, usaremos `@react-native-community/async-storage`, que é um banco assíncrono
```bash
yarn add @react-native-community/async-storage

cd ios && pod install && cd ..
```

Em `src/hooks/auth.tsx`, como `AsyncStorage` é assíncrono, não conseguimos pegar as informações que já estavam armazenadas dentro do nosso storage como no web.
```tsx
import React, {
  createContext,
  useCallback,
  useState,
  useContext,
  useEffect,
} from 'react';
import AsyncStorage from '@react-native-community/async-storage';
import api from '../services/api';

interface AuthState {
  token: string;
  user: object;
}

interface SignInCredentials {
  email: string;
  password: string;
}

interface AuthContextData {
  user: object;
  signIn(credentials: SignInCredentials): Promise<void>;
  signOut(): void;
}

const AuthContext = createContext<AuthContextData>({} as AuthContextData);

const AuthProvider: React.FC = ({ children }) => {
  const [data, setData] = useState<AuthState>({} as AuthState);

  useEffect(() => {
    async function loadStorageData(): Promise<void> {
      const [token, user] = await AsyncStorage.multiGet([
        '@GoBarber:token',
        '@GoBarber:user',
      ]);

      if (token[1] && user[1]) {
        setData({ token: token[1], user: JSON.parse(user[1]) });
      }
    }

    loadStorageData();
  }, []);

  const signIn = useCallback(async ({ email, password }) => {
    const response = await api.post('sessions', {
      email,
      password,
    });

    const { token, user } = response.data;

    await AsyncStorage.multiSet([
      ['@GoBarber:token', token],
      ['@GoBarber:user', JSON.stringify(user)],
    ]);

    setData({ token, user });
  }, []);

  const signOut = useCallback(async () => {
    await AsyncStorage.multiRemove(['@GoBarber:token', '@GoBarber:user']);

    setData({} as AuthState);
  }, []);

  return (
    <AuthContext.Provider value={{ user: data.user, signIn, signOut }}>
      {children}
    </AuthContext.Provider>
  );
};

function useAuth(): AuthContextData {
  const context = useContext(AuthContext);

  if (!context) {
    throw new Error('useAuth must be used within an AuthProvider');
  }

  return context;
}

export { AuthProvider, useAuth };
```

E também precisaremos do hook `src/hooks/index.tsx`
```tsx
import React from 'react';

import { AuthProvider } from './auth';

const AppProvider: React.FC = ({ children }) => {
  return <AuthProvider>{children}</AuthProvider>;
};

export default AppProvider;
```

E importamos no App para colocar em volta da nossa aplicação
```tsx
import AppProvider from './hooks';

const App: React.FC = () => (
  <NavigationContainer>
    <StatusBar barStyle="light-content" backgroundColor="#312e38" />
    <AppProvider>
      <View style={{ flex: 1, backgroundColor: '#312e38' }}>
        <Routes />
      </View>
    </AppProvider>
  </NavigationContainer>
);
```

Então, em SignIn, importamos nosso hook useAuth
```tsx
import { useAuth } from '../../hooks/auth';
//..
  const { signIn } = useAuth();
  //...
      await signIn({
        email: data.email,
        password: data.password,
      });
```

E uma forma de ver se o singIn está funcionando é dar um `console.log` do user que vem de `useAuth`
```tsx
  const { signIn, user } = useAuth();
  console.log(user);
```

## Rotas privadas
Vamos mudar o nome do arquivo `src/routes/index.tsx` para `src/routes/auth.routes.tsx` e criamos um novo `src/routes/index.tsx`
```tsx
import React from 'react';
import AuthRoutes from './auth.routes';

const Routes: React.FC = () => {
  return <AuthRoutes />;
};

export default Routes;
```

Vamos criar um `src/pages/Dashboard/index.tsx`
```tsx
import React from 'react';
import { View, Button } from 'react-native';

import { useAuth } from '../../hooks/auth';

const Dashboard: React.FC = () => {
  const { signOut } = useAuth();

  return (
    <View style={{ flex: 1, justifyContent: 'center' }}>
      <Button title="Sair" onPress={signOut} />
    </View>
  );
};

export default Dashboard;
```

E também criaremos um `src/routes/app.routes.tsx` para quando o user estiver autenticado, jogar para Dashboard
```tsx
import React from 'react';
import { createStackNavigator } from '@react-navigation/stack';

import Dashboard from '../pages/Dashboard';

const App = createStackNavigator();

const AppRoutes: React.FC = () => (
  <App.Navigator
    screenOptions={{
      // headerShown: false,
      cardStyle: { backgroundColor: '#312e38' },
    }}
  >
    <App.Screen name="Dashboard" component={Dashboard} />
  </App.Navigator>
);

export default AppRoutes;
```

Quando damos reload, vemos rapidamente a tela de login aparecer pois o AsyncStorage demora um tempinho para carregar. Para contornar isso, vamos no `hooks` de `auth` para colocar um `state` de `loading`
```tsx
interface AuthContextData {
  user: object;
  loading: boolean;
  signIn(credentials: SignInCredentials): Promise<void>;
  signOut(): void;
}
//...
  const [loading, setLoading] = useState(true);
//...
  useEffect(() => {
      //...
      setLoading(false);
    }
    loadStorageData();
  }, []);
  //...
    <AuthContext.Provider value={{ user: data.user, loading, signIn, signOut }}>
```

Nas rotas, vamos verificar se o user está logado ou não, e enquanto estiver com loading, vamos manter `ActivityIndicator`
```tsx
const Routes: React.FC = () => {
  const { user, loading } = useAuth();

  if (loading) {
    return (
      <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
        <ActivityIndicator size="large" color="#999" />
      </View>
    );
  }
  return user ? <AppRoutes /> : <AuthRoutes />;
};
```

---

Continuar em [Finalizando App](README2.md)
