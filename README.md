# React Native

É uma versão do React para desenvolvimento mobile.
O mesmo código roda no Android e no iOS e manipular de forma diferente.
Injeta o JS core dentro do nosso celular e roda sem transpilar.

*Arquitetura:*
JS -> Metro Bundler (packager) -> Bundle -> Bridge -> Android | iOS

Metro Bundler: seria como nosso Webpack do ReactJS.
Bundle: nosso arquivo JS.
Bridge: é a ponte entre nosso código JS em código nativo.

Sintaxe com declaração de componente similar ao web, mas não usamos HTML mas componentes próprios.

`<View>` é como a `<div>`

`<Text>` é como o `<p>`

`<TouchableOpacity>` é como `<button>`

Como são componentes próprios, nenhum vem com estilização e não é estilizado com `class` ou `id`. Precisa da propriedade `style` e a sintaxe é tudo `css`.

Yoga é uma biblioteca que converte estilo `css` em Objective-C e Java.

Expo é um SDK para usar as funcionalidades do React Native.
Não precisa instalar emulador e é só instalar nosso aplicativo dentro do Expo e ele roda no nosso celular. A desvantagem é que tem limitação sobre o código nativo. Se precisar mexer com Objective-C ou Java, não vai conseguir pelo Expo. Além disso, várias bibliotecas não tem suporte para o Expo.

Instalar SDK do Android e do iOS. (https://react-native.rocketseat.dev/)

(Instalação do XCode foi demorada para conseguir rodar o Emulador de iOS)

## Inicializando um projeto
```bash
react-native init mobile
```

Abrir no VSCode.

Excluir arquivos `.eslintrc.js`, `prettierrc.js`

Rodar o projeto
```bash
react-native run-ios --simulator "iPhone 11 Pro Max"
```

Deu erro porque XCode não estava instalado corretamente. Depois de instalar novamente pela Apple, com login e tudo mais, instalei os pods do projeto.
```bash
cd ios && pod install
```

Voltei para raiz do projeto e rodei novamente
```bash
react-native run-ios --simulator "iPhone 11 Pro Max"
```

Depois de 6 minutos, rodou 🎉🎉🎉🎉🎉 e abriu o Metro Bundle que fica ouvindo todas alterações do código e constrói um bundle que roda e atualiza a tela no emulador.

No Mac, `Cmd + R` faz o reload no emulador e `Cmd + D` abre o developer menu.

Ou no Metro bundle também pode digitar `r` para relod ou `d` para developer menu.

Deleta o arquivo `App.js` e cria um arquivo `src/index.js`.
```js
import React from 'react'
import { View } from 'react-native'

export default function App() {
  return <View />
}
```

E altera a importação dentro do `index.js` da raiz do projeto.
```js
import App from './src/index';
```

Os elementos do React Native não possuem valor semântico, nem estilização própria. E todos já possuem por padrão `display: flex`

View: div, header, footer, nav, aside, main, section.

Text: p, span, strong, h1, h2, h3.

Importar `StyleSheet` do `react-native`. E não precisa criar um arquivo `.css`. Todo `css` é feito direto dentro do arquivo `.js`. E agora as propriedades de estilo são escritas em camel case.
```js
import React from 'react'
import { View, StyleSheet } from 'react-native'

export default function App() {
  return <View style={styles.container} />
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#7159c1',
  },
})
```

No React Native não temos herança de estilos. Se colocar cor no `styles.container`, o estilo não será repassado para seus filhos.

É necessário criar um estilo próprio para cada tag de elemento.
```js
import React from 'react'
import { View, Text, StyleSheet } from 'react-native'

export default function App() {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Hello GoStack</Text>
    </View>
  )
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#7159c1',
    justifyContent: 'center',
    alignItems: 'center',
  },
  title: {
    color: '#fff',
    fontSize: 32,
    fontWeight: 'bold',
  },
})
```

Podemos estilizar a `StatusBar` importando pelo `react-native` e também adicionando a `barStyle` que vai alterar a cor da fonte e o `backgroundColor` que é para Android.
```js
import React from 'react'
import { View, Text, StyleSheet, StatusBar } from 'react-native'

export default function App() {
  return (
    <>
      <StatusBar barStyle="light-content" backgroundColor="#7159c1" />
      
      <View style={styles.container}>
        <Text style={styles.title}>Hello GoStack</Text>
      </View>
    </>
  )
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#7159c1',
    justifyContent: 'center',
    alignItems: 'center',
  },
  title: {
    color: '#fff',
    fontSize: 32,
    fontWeight: 'bold',
  },
})
```

*Fazendo request para o Back-end*

Rodar o backend.
```bash
cd backend
yarn dev
```

Criar 2 projetos para termos o que listar.

Voltar para o projeto mobile e adicionar o pacote `axios`
```bash
yarn add axios
```

Criar o arquivo `src/services/api`. Detalhes para a `baseURL` pois vai depender de onde está acessando o projeto.

- iOS com emulador: localhost
- iOS com físico: IP da máquina
- Android com emulador: 
  - localhost utilizando adb reverse no terminal `adb reverse tcp:3333 tcp:3333`
  - 10.0.2.2 (Android Studio)
  - 10.0.3.2 (Android Genymotion)
- Android com físico: IP da máquina

```js
import axios from 'axios'

const api = axios.create({
  baseURL: 'http://localhost:3333'
})

export default apiimport axios from 'axios'

const api = axios.create({
  baseURL: 'http://localhost:3333'
})

export default api
```

Importar `api`, `useState` e `useEffect` e fazer o mesmo que fizemos no web.
```js
import React, { useEffect, useState } from 'react'

import api from './services/api'

export default function App() {
  const [projects, setProjects] = useState([])

  useEffect(() => {
    api.get('projects').then(response => {
      console.log(response.data)
      setProjects(response.data)
    })
  }, [])

  ...
}
```

O `console.log` é mostrado no terminal do Metro bundle. Ou no emulador, `Cmd + d` e clica na opção `Debug`. Vai abrir uma aba do Chrome e é só inspecionar.

Adicionando a lista de projetos:
```js
import React, { useEffect, useState } from 'react'
import { View, Text, StyleSheet, StatusBar } from 'react-native'

import api from './services/api'

export default function App() {
  const [projects, setProjects] = useState([])

  useEffect(() => {
    api.get('projects').then(response => {
      setProjects(response.data)
    })
  }, [])
  return (
    <>
      <StatusBar barStyle="light-content" backgroundColor="#7159c1" />

      <View style={styles.container}>
        {projects.map(project => (
          <Text key={project.id} style={styles.project}>{project.title}</Text>
        ))}
      </View>
    </>
  )
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#7159c1',
    justifyContent: 'center',
    alignItems: 'center',
  },
  project: {
    color: '#fff',
    fontSize: 20,
  },
})
```

Quando precisar dar scroll na tela, ao invés do `View` podemos usar `ScrollView`.
Mas nesse caso, utilizaremos a `FlatList` pois precisamos do scroll especificamente para lista.
```js
import React, { useEffect, useState } from 'react'
import { Text, StyleSheet, StatusBar, FlatList } from 'react-native'

import api from './services/api'

export default function App() {
  const [projects, setProjects] = useState([])

  useEffect(() => {
    api.get('projects').then(response => {
      setProjects(response.data)
    })
  }, [])
  return (
    <>
      <StatusBar barStyle="light-content" backgroundColor="#7159c1" />

      <FlatList
        style={styles.container}
        data={projects}
        keyExtractor={project => project.id}
        renderItem={({ item: project }) => (
          <Text style={styles.project}>{project.title}</Text>
        )}
      />
    </>
  )
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#7159c1',
  },
  project: {
    color: '#fff',
    fontSize: 30,
  },
})
```

Mas como removemos o `justifyContent` e `alignItems`, os item ficaram todos em cima da `statusBar`. Então, colocamos o `SafeAreaView` em volta da `FlatList`.
```js
import React, { useEffect, useState } from 'react'
import { SafeAreaView, Text, StyleSheet, StatusBar, FlatList } from 'react-native'

import api from './services/api'

export default function App() {
  const [projects, setProjects] = useState([])

  useEffect(() => {
    api.get('projects').then(response => {
      setProjects(response.data)
    })
  }, [])
  return (
    <>
      <StatusBar barStyle="light-content" backgroundColor="#7159c1" />

    <SafeAreaView style={styles.container}>
      <FlatList
        data={projects}
        keyExtractor={project => project.id}
        renderItem={({ item: project }) => (
          <Text style={styles.project}>{project.title}</Text>
        )}
      />
    </SafeAreaView>
    </>
  )
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#7159c1',
  },
  project: {
    color: '#fff',
    fontSize: 30,
  },
})
```

*Fazendo post na api*

Existe o componente `Button` do `react-native` mas ele é um dos únicos que já vem com um estilo próprio.
Por isso, usaremos o `TouchableOpacity` pois diminui levemente a opacidade do botão ao ser clicado.
E o request da api é o mesmo que do web.
```js
import React, { useEffect, useState } from 'react'
import { SafeAreaView, Text, StyleSheet, StatusBar, FlatList, TouchableOpacity } from 'react-native'

import api from './services/api'

export default function App() {
  const [projects, setProjects] = useState([])

  useEffect(() => {
    api.get('projects').then(response => {
      setProjects(response.data)
    })
  }, [])

  async function handleAddProject() {
    const response = await api.post('projects', {
      title: `Novo projeto ${Date.now()}`,
      owner: 'Cintia Yamamoto',
    })
    const project = response.data
    setProjects([ ...projects, project ])
  }

  return (
    <>
      <StatusBar barStyle="light-content" backgroundColor="#7159c1" />

    <SafeAreaView style={styles.container}>
      <FlatList
        data={projects}
        keyExtractor={project => project.id}
        renderItem={({ item: project }) => (
          <Text style={styles.project}>{project.title}</Text>
        )}
      />

      <TouchableOpacity
        activeOpacity={0.6}
        style={styles.button}
        onPress={handleAddProject}
      >
        <Text style={styles.buttonText}>Adicionar projeto</Text>
      </TouchableOpacity>
    </SafeAreaView>
    </>
  )
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#7159c1',
  },
  project: {
    color: '#fff',
    fontSize: 30,
  },
  button:{
    backgroundColor: '#fff',
    alignSelf: 'stretch',
    margin: 20,
    height: 50,
    borderRadius: 4,
    justifyContent: 'center',
    alignItems: 'center',
  },
  buttonText: {
    fontWeight: 'bold',
    fontSize: 16,
  },
})
```