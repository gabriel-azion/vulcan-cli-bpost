# Atinja o próximo nível na implatação Edge: adotando Polyfills do Node.js (1)
# Desbloqueie a implatação Edge sem esforço com Vulcan e Polyfills do Node.js (2)
# Desbloqueie a implatação Edge sem esforço com Vulcan e Polyfills do Node.js (3)

Como desenvolvedores, estamos familiarizados com o mundo acelerado e em constante mudança dos frameworks web. Às vezes, pode ser confuso, pois se destacar no uso de um framework específico requer um entendimento mais profundo que vai além de apenas conhecer a linguagem de programação. Um dos obstáculos significativos é configurar o projeto para funcionar perfeitamente em diferentes ambientes.

Questões como "Isso vai funcionar na nuvem?" e "Em qual plataforma deve ser executado?" surgem frequentemente. Mas e se você pudesse implementar seu projeto no edge, aproveitando a baixa latência, segurança e disponibilidade sem se preocupar com o ambiente específico? Este post explora o conceito de adaptar projetos para serem executados na borda e apresenta uma solução que simplifica o processo.

## Adaptando Projetos para Execução na Borda

Para garantir uma implatação contínua, um projeto deve ser estruturado de maneira a evitar atritos com a plataforma hospedeira. Não é incomum encontrar situações em que as configurações necessárias para executar um aplicativo em uma plataforma se parecem muito pouco com as necessárias para uma plataforma diferente. Isso pode levar a um bloqueio do fornecedor, onde a troca de hosts se torna cara em termos de tempo e dinheiro, forçando os clientes a continuar com o provedor de serviços atual.

Na Azion, valorizamos o poder do software de código aberto e temos como objetivo capacitar o desenvolvimento web moderno. Nossa plataforma permite a inicialização e implatação de projetos em vários frameworks web, incluindo:

- Next.js
- React
- Vue
- Astro
- Angular
- Hexo
- Vite
- JavaScript em si

A Azion fornece seu próprio [Edge Runtime]() projetado para fornecer uma experiência ideal para executar aplicativos na borda. Este runtime alimenta as [Edge Functions da Azion]() e abre um mundo de possibilidades para desenvolvedores e empresas. Além disso, desenvolvemos o [Vulcan]() para preencher as lacunas e permitir que os frameworks web sejam executados nativamente na borda. O Vulcan simplifica a integração de *polyfills* para a Computação de Borda, revolucionando o processo de criação de Workers, especialmente para a plataforma Azion.

> Se você está curioso sobre o que são *polyfills*, são trechos de código usados no JavaScript para fornecer funcionalidades modernas aos ambientes que não oferecem suporte nativo. Os Polyfills preenchem as lacunas, garantindo um comportamento consistente entre diferentes navegadores. Por exemplo, vamos considerar um runtime que não oferece suporte a uma API específica do Node.js, na qual um projeto depende. Durante o processo de construção, o Vulcan reconhece a assinatura dessa API e a substitui por uma API relativa, eliminando a necessidade de adaptação manual do projeto.

O Vulcan se destaca na criação de um protocolo intuitivo e simplificado para a criação de *presets*. Este recurso aprimora a personalização e permite que os desenvolvedores adaptem seus aplicativos aos requisitos exclusivos do projeto, fornecendo a flexibilidade necessária para otimizá-los de maneira eficaz.

## Configurando um Projeto

O [CLI da Azion]() é uma ferramenta excepcional que melhora muito a experiência do desenvolvedor. Com o CLI instalado em seu ambiente, iniciar um projeto é tão simples quanto executar o seguinte comando:

```bash
azion init
```
Este comando inicia uma jornada interativa onde você pode escolher o *template* desejado.

Depois de selecionar o *template*, cada framework apresentará um conjunto específico de etapas. Escolha executar o projeto localmente e instale as dependências necessárias.

### Aproveitando Polyfills

O Vulcan torna possível o uso de *polyfills*. Vamos dar uma olhada mais de perto em como configurar um projeto para utilizar esse recurso.

**Exemplo**: Supondo que você queira iniciar um projeto JavaScript que utiliza a API Buffer do Node.js. Para conseguir isso, você precisa informar ao Vulcan que o projeto implementa polyfills.

O Vulcan lê um arquivo de configuração chamado `vulcan.config.js`. Crie este arquivo e inclua as seguintes propriedades:

```js
module.exports = {
  entry: 'main.js',
  builder: 'webpack',
  useNodePolyfills: true,
};
```

- **entry**: Representa o ponto de entrada principal do seu aplicativo, onde o processo de construção começa. Não é aplicável para soluções Jamstack.
- **builder**: Define a ferramenta de construção a ser usada, `esbuild` ou `webpack`.
- **useNodePolyfills**: Especifica se os polyfills do Node.js devem ser aplicados.

Depois de aplicar essas configurações, você pode importar as APIs necessárias para o seu projeto. Por exemplo, vamos considerar a importação do Buffer de Node.js:

**Dentro de main.js**:

```js
// Importa a classe Buffer do módulo 'buffer' no Node.js
import { Buffer } from 'node:buffer';

// Define uma função chamada 'myWorker' que leva um evento como argumento
export default function myWorker(event) {
  // Cria uma nova instância de Buffer 'buf1' a partir da string "x"
  var buf1 = Buffer.from("x");
  // Cria uma nova instância de Buffer 'buf2' a partir da string "x"
  var buf2 = Buffer.from("x");
  // Comparando 'buf1' e 'buf2' usando o método Buffer.compare,
  // que retorna um número indicando a igualdade dos buffers
  var a = Buffer.compare(buf1, buf2);

  // O resultado será 0, pois ambos os buffers são iguais
  console.log(a);

  // Agora, vamos trocar os valores de 'buf1' e 'buf2'
  buf1 = Buffer.from("y");
  buf2 = Buffer.from("x");
  // Comparando 'buf1' e 'buf2' novamente
  a = Buffer.compare(buf1, buf2);

  // Aqui, o resultado será 1
  console.log(a);

  // A função retorna um novo objeto Response com a string "Testando polyfills de buffer"
  return new Response("Testando polyfills");
}
```

Para executar o projeto localmente, use o seguinte comando:

```bash 
azion dev
```

Agora você pode acessar o projeto localmente.

> Nota: Certifique-se de que está no diretório raiz do seu projeto para que os comandos sejam executados corretamente.

O comando `azion dev` inicia o **processo de construção**, que é gerenciado de maneira transparente pelo Vulcan e retorna a porta para acessar o projeto.

O projeto de exemplo usado neste exemplo pode ser encontrado no repositório [Azion Samples](https://github.com/aziontech/azion-samples/tree/dev/samples/polyfills/buffer) no GitHub.

---

## Runtime Edge, Vulcan e CLI Azion

![cli - vulcan - runtime ](cli-vulcan-runtime.png)

### Processo explicado 

1. O usuário inicia o processo executando `azion` ou `azion init` via CLI Azion e seleciona o *template* desejado.
2. CLI Azion, então, invoca o Vulcan, que se encarrega de inicializar o projeto.
3. Uma vez inicializado, o Vulcan devolve o projeto ao usuário, que decide se implementa o projeto ou o executa localmente.
4. Se o usuário optar por executar o projeto localmente, Vulcan dispara o processo de construção e gera o worker.
5. Se o usuário decidir implementar o projeto, ele executa o comando `azion deploy`. Isso inicia o processo de construção novamente, após o qual CLI Azion cria um aplicativo de borda e o implementa na rede edge distribuída da Azion.
6. Após a implatação bem-sucedida, o CLI Azion fornece o domínio do aplicativo. Depois de um breve período de espera, o aplicativo se torna acessível.

### Responsabilidades 

**CLI Azion**: a interface de linha de comando que serve como principal ponto de interação entre o usuário e o sistema. Gerencia todo o processo de implatação do aplicativo, garantindo um fluxo de trabalho suave e eficiente.

**Vulcan**: o motor que impulsiona a inicialização do projeto, construção e adaptação. Adequa inteligentemente o projeto com base no *template* selecionado, garantindo que o aplicativo esteja otimamente configurado para o uso pretendido.

**Runtime Edge Azion**: o ambiente de execução que hospeda o aplicativo e gerencia sua execução. O aplicativo é distribuído através da rede global de Nós Edge da Azion, garantindo que ele esteja sempre perto dos usuários, maximizando assim velocidade e eficiência.

---

Ao utilizar a plataforma Azion, os desenvolvedores podem desfrutar de uma melhor experiência de implatação de frameworks web na borda. Adapte seus projetos para serem executados de maneira contínua enquanto se concentra na funcionalidade principal, em vez de se preocupar com o ambiente específico. Com o Runtime Edge da Azion e a ferramenta Vulcan, os desenvolvedores ganham o poder de otimizar seus aplicativos de maneira eficaz, aproveitando a baixa latência, segurança e disponibilidade no edge.
