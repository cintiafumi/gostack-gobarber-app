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
