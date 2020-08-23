[↩ Voltar](README2.md)

# Testes no React Native

## Configurando ambiente
Instalar a lib
```bash
yarn add -D react-native-testing-library
yarn add -D @testing-library/jest-native
```

E adicionamos no package as configurações
```json
{
  "jest": {
    "setupFilesAfterEnv": ["@testing-library/jest-native/extend-expect"],
    "setupFiles": [
      "./src/setupTests.ts",
      "./node_modules/react-native-gesture-handler/jestSetup.js"
    ],
    "collectCoverageFrom": [
      "src/pages/**/*.tsx",
      "src/components/**/*.tsx",
      "src/hooks/*.tsx",
      "!src/hooks/index.tsx"
    ],
  }
}
```

E no eslint também adicionamos o jest
```json
{
  "jest": true
}
```

Criamos um arquivo `src/setupTests.ts` para mockar o AsyncStorage
```ts
import mockAsyncStorage from '@react-native-community/async-storage/jest/async-storage-mock';

jest.mock('@react-native-community/async-storage', () => mockAsyncStorage);
```

E o teste da tela de SignIn
```tsx
import React from 'react';
import { render } from 'react-native-testing-library';

import SignIn from '../../pages/SignIn';

jest.mock('@react-navigation/native', () => {
  return {
    useNavigation: jest.fn(),
  };
});

describe('SignIn page', () => {
  it('should contain email/password inputs', () => {
    const { getByPlaceholder } = render(<SignIn />);

    expect(getByPlaceholder('E-mail')).toBeTruthy();
    expect(getByPlaceholder('Senha')).toBeTruthy();
  });
});
```
