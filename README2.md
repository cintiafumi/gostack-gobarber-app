[↩ Voltar](README.md)

# Finalizando App

O layout ficaria bem complicado de seguir conforme o Figma, então, iremos usar a lib `react-native-community/datetimepicker` para fazer o calendário. E a lib `react-native-community/react-native-image-picker` para tirarmos foto de perfil.

## Ajustes na API (Backend)
Fizemos alguns ajustes no backend.

- Utilizando Query Params
- Agendamentos no mesmo horário
- Dias indisponíveis no mês
- Cliente dos agendamentos

## Criando páginas e rotas
Criamos somente a estrutura das páginas `AppointmentCreated`, `CreateAppointment`, `Profile` e já deixamos as rotas prontas.
```tsx
const AppRoutes: React.FC = () => (
  <App.Navigator
    screenOptions={{
      headerShown: false,
      cardStyle: { backgroundColor: '#312e38' },
    }}
  >
    <App.Screen name="Dashboard" component={Dashboard} />
    <App.Screen name="CreateAppointment" component={CreateAppointment} />
    <App.Screen name="AppointmentCreated" component={AppointmentCreated} />

    <App.Screen name="Profile" component={Profile} />
  </App.Navigator>
);
```

## Header do Dashboard
```tsx
import React, { useCallback } from 'react';
import { useNavigation } from '@react-navigation/native';

import { useAuth } from '../../hooks/auth';

import {
  Container,
  Header,
  HeaderTitle,
  UserName,
  ProfileButton,
  UserAvatar,
} from './styles';

const Dashboard: React.FC = () => {
  const { signOut, user } = useAuth();
  const { navigate } = useNavigation();

  const navigateToProfile = useCallback(() => {
    navigate('Profile');
  }, [navigate]);

  return (
    <Container>
      <Header>
        <HeaderTitle>
          Bem vindo,
          {'\n'}
          <UserName>{user.name}</UserName>
        </HeaderTitle>

        <ProfileButton onPress={navigateToProfile}>
          <UserAvatar source={{ uri: user.avatar_url }} />
        </ProfileButton>
      </Header>
    </Container>
  );
};

export default Dashboard;
```

```ts
import styled from 'styled-components/native';
import { getStatusBarHeight } from 'react-native-iphone-x-helper';

export const Container = styled.View`
  flex: 1;
`;

export const Header = styled.View`
  padding: 24px;
  padding-top: ${getStatusBarHeight() + 24}px;
  background: #28262e;

  flex-direction: row;
  justify-content: space-between;
  align-items: center;
`;

export const HeaderTitle = styled.Text`
  color: #f4ede8;
  font-size: 20px;
  font-family: 'RobotoSlab-Regular';
  line-height: 28px;
`;

export const UserName = styled.Text`
  color: #ff9000;
  font-family: 'RobotoSlab-Medium';
`;

export const ProfileButton = styled.TouchableOpacity``;

export const UserAvatar = styled.Image`
  width: 56px;
  height: 56px;
  border-radius: 28px;
`;
```

## Buscando providers da api
Temos que adicionar a interface `User` no nosso arquivo `auth` e o `authorization` no header das requisições
```ts
interface User {
  id: string;
  name: string;
  email: string;
  avatar_url: string;
}
//...
  useEffect(() => {
    async function loadStorageData(): Promise<void> {
      //...
      if (token[1] && user[1]) {
        api.defaults.headers.authorization = `Bearer ${token[1]}`;

        setData({ token: token[1], user: JSON.parse(user[1]) });
      }
      //...

  const signIn = useCallback(async ({ email, password }) => {
    //...
    api.defaults.headers.authorization = `Bearer ${token}`;
```

Adicionamos a interface `Provider` para então tipar o que trazermos da api, e fizemos o componente `ProviderList`
```tsx
export interface Provider {
  id: string;
  name: string;
  avatar_url: string;
}

const Dashboard: React.FC = () => {
  const [providers, setProviders] = useState<Provider[]>([]);
  //...
  useEffect(() => {
    api.get('/providers').then((response) => {
      setProviders(response.data);
    });
  }, []);
  //...
      <ProvidersList
        data={providers}
        keyExtractor={(provider) => provider.id}
        renderItem={({ item }) => <UserName>{item.name}</UserName>}
      />
```

Nos styles, o `ProvidersList` virá de um `FlatList`
```ts
export const ProvidersList = styled(FlatList as new () => FlatList<Provider>)``;
```
