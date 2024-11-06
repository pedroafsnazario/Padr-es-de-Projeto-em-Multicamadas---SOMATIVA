# Padrões de Projeto em Multicamadas SOMATIVA 💻

- Aluno: Pedro Antonio Fernandes dos Santos Nazario
- Curso: Sistemas de Informação
- Faculdade: Pontifícia Universidade Católica (PUC)

## Indíce📖
* [Somativa](#somativa)
* [Projeto Pokedex](#projeto-pokedex)
* [Conclusão](#conclusão)

## Somativa📰

### 1. Escolha um projeto antigo que você tenha desenvolvido.👨‍💻
 #### Projeto escolhido -> Pokedex feita em Kotlin!💠
 * Evite programas de um arquivo só❗
 * Pode ser em qualquer linguagem✅

 
### 2. Identifique pelo menos 5 pontos de melhoria✔

* Explique por que você considera que aquele ponto precisa ser melhorado
* Diga que padrão de projetos você utilizaria para realizar a melhoria
* Mostre um protótipo de como você imagina que o código final ficaria, tanto do ponto de vista da implementação, quanto do uso desse código
* No total, você deve identificar pelo menos 3 padrões de projeto diferentes. Não sugira melhorias sempre usando o mesmo padrão!

### 3. A entrega será feita através do github.

* Crie um repositório com seu código original (não precisa das modificações).
* Envie o link para o repositório.
* Lembre-se de deixar o repositório publico.

### 4. Formato da entrega

* Você pode entregar o trabalho tanto com em vídeo, quanto em texto. Escolha um dos dois formatos
  - Entrega será feita em documento no formato word
* Se for entregar o vídeo, adicione um link no README.md do seu projeto para o link do youtube para um conteúdo não listado. Não envie o zip do vídeo.

## Projeto Pokedex👾
* Foi um projeto que realizei semestre passado na matéria de Android com o professor Mark. O objetivo era realizar um projeto completo em android (no caso, um aplicativo) para o trabalho final da matéria, como o tema para o aplicativo era livre, resolvi criar um aplicativo que funcionasse como uma Pokedex.
* No entanto o projeto final apesar de ter uma "Estrutura" boa (Pois utilizei alguns tutoriais para criar a pokedex). Seu acabamento ficou péssimo, a visualização se tornou muito simplória e funcionalidades mínimas. Em resumo, eu poderia ter feito o projeto mais completo, mas no fim ficou horrendo e como dizem, "foi feito de qualquer jeito".
* Não tinha muita experiencia para programar em Kotlin e muito menos programar um aplicativo completo integrado com API`S, por isso o projeto ficou do jeito que está😅
* Por meio desta Somativa, irei demonstrar como o Projeto poderia ter ficado melhor se eu tivesse produzido/abordado ele com Padròes de projeto distintos e citar melhorias que poderiam ser feitas.
* As melhorias que demonstrarei são bases que usaria pro código, tendo em mente que, ao invez de criar 'Pokemon Lists' eu iria implementar uma API de pokemon que continha uma lista com todos os Pokemons da região de Kanto da série.
  
### Pontos de Melhoria

### Adicionar Funcionalidades de Pesquisa e Filtro🔍
* Atualmente, a Pokédex só exibe uma lista de Pokémon. Seria interessante incluir funcionalidades como busca por nome e filtros (por tipo, força, etc.) para melhorar a navegação.
* Com isso, o melhor padrão a se usar neste quesito seria o Strategy. Pois permite que você defina uma família de algoritmos, como diferentes critérios de busca e filtragem, e os torne intercambiáveis. Assim, é possível adicionar novos filtros facilmente sem modificar o código da interface ou dos dados.
* Usando o Strategy para melhorar este ponto do meu projeto, eu poderia ter montado a base para pesquisa da seguinte forma:
  ```Kotlin
  // Pesquisa e Filtro de Pokemons
          interface FilterStrategy {
  // Implementação da API por meio do Strategy
    fun filter(pokemonList: List<Pokemon>): List<Pokemon>
  }
  //Filtro de Pokemons
  class TypeFilter(private val type: String) : FilterStrategy {
    override fun filter(pokemonList: List<Pokemon>): List<Pokemon> {
        return pokemonList.filter { it.type == type }
    }
  }
 
  class NameFilter(private val name: String) : FilterStrategy {
    override fun filter(pokemonList: List<Pokemon>): List<Pokemon> {
        return pokemonList.filter { it.name.contains(name, ignoreCase = true) }
    }
  }
  ```
### Mostrar Detalhes de Cada Pokémon📜
* Ao clicar em um Pokémon na lista, o usuário poderia ver detalhes adicionais, como habilidades, estatísticas e evoluções. Isso tornaria a aplicação mais interativa e informativa.
* Com o padrão Observer pode ser utilizado para notificar automaticamente a interface da Pokédex sobre a seleção de um Pokémon. Cada vez que o usuário clica em um Pokémon, o Observer notificará os componentes de interface necessários para exibir os detalhes atualizados.

```Kotlin
interface PokemonSelectionObserver {
    fun onPokemonSelected(pokemon: Pokemon)
}

class PokedexView : PokemonSelectionObserver {
    override fun onPokemonSelected(pokemon: Pokemon) {
        // Exibe detalhes do Pokémon selecionado
    }
}

class PokemonNotifier {
    private val observers = mutableListOf<PokemonSelectionObserver>()
    
    fun addObserver(observer: PokemonSelectionObserver) {
        observers.add(observer)
    }
    
    fun notifySelection(pokemon: Pokemon) {
        observers.forEach { it.onPokemonSelected(pokemon) }
    }
 }

```
### Adicionar Layout e Design Mais Elaborado🎨
* A interface atual está absurdamente simples e sem atrativos visuais. Um design aprimorado, como um layout de grade com imagens e uma interface mais interativa, melhoraria bastante a experiência do usuário(e a qualidade do aplicativo).
* Por isso, eu usaria o padrão Decorator neste caso para adicionar funcionalidades ou melhorias visuais dinamicamente, sem alterar a classe principal. Isso é útil para construir uma interface modular, onde você pode adicionar estilos, efeitos visuais, ou até mesmo funcionalidades como animações.
```Kotlin
interface PokemonCard {
    fun display()
}

class BasicPokemonCard(private val pokemon: Pokemon) : PokemonCard {
    override fun display() {
        // Exibe Pokémon básico
    }
}

class ImageDecorator(private val card: PokemonCard) : PokemonCard {
    override fun display() {
        card.display()
        // Adiciona imagem ao Pokémon
    }
}

class FancyBorderDecorator(private val card: PokemonCard) : PokemonCard {
    override fun display() {
        card.display()
        // Adiciona bordas e estilização
    }
}
```
### Organizar o Código da Interface e Dados de Forma mais Modular📚
* É fundamental que o código esteja organizado para facilitar a adição de novas funcionalidades no futuro, como o MVC, que separa a interface da lógica de dados.
* o MVC facilita a manutenção e expansão do código separando a lógica em camadas bem definidas. No seu caso, a camada Model para dados de Pokémon, View para a exibição da lista, e Controller para a manipulação de dados e respostas a ações do usuário ajudariam muito.
```Kotlin
// Model
data class Pokemon(val id: Int, val name: String, val type: String, val power: Int)

// View
class PokedexView {
    fun displayPokemonList(pokemonList: List<Pokemon>) {
        pokemonList.forEach { pokemon ->
            println("ID: ${pokemon.id}, Nome: ${pokemon.name}, Tipo: ${pokemon.type}, Força: ${pokemon.power}")
        }
    }

    fun displayPokemonDetails(pokemon: Pokemon) {
        println("Detalhes do Pokémon: ${pokemon.name}")
        println("ID: ${pokemon.id}, Tipo: ${pokemon.type}, Força: ${pokemon.power}")
    }
}

// Controller
class PokedexController(private val view: PokedexView, private val pokemonList: List<Pokemon>) {

    fun showAllPokemon() {
        view.displayPokemonList(pokemonList)
    }

    fun showPokemonById(id: Int) {
        val pokemon = pokemonList.find { it.id == id }
        if (pokemon != null) {
            view.displayPokemonDetails(pokemon)
        } else {
            println("Pokémon com ID $id não encontrado.")
        }
    }
}
```
### Adicionar Opções de Ordenação para a Lista de Pokémon⚙
* A adição de diferentes formas de ordenação (por nome, tipo, força) melhora a experiência do usuário ao explorar a lista.
* usando uma implementação similar à do ponto 1 (O Strategy). O mesmo padrão de filtragem pode ser adaptado para ordenação, com diferentes estratégias para ordenar a lista de Pokémon conforme critérios específicos.
```Kotlin
// Strategy
interface SortStrategy {
    fun sort(pokemonList: List<Pokemon>): List<Pokemon>
}
class SortByName : SortStrategy {
    override fun sort(pokemonList: List<Pokemon>): List<Pokemon> {
        return pokemonList.sortedBy { it.name }
    }
}

class SortByType : SortStrategy {
    override fun sort(pokemonList: List<Pokemon>): List<Pokemon> {
        return pokemonList.sortedBy { it.type }
    }
}

class SortByPower : SortStrategy {
    override fun sort(pokemonList: List<Pokemon>): List<Pokemon> {
        return pokemonList.sortedByDescending { it.power }
    }
}
// Integração com o Controller

class PokedexControllerWithSort(
    private val view: PokedexView,
    private val pokemonList: List<Pokemon>
) {
    private var sortStrategy: SortStrategy = SortByName() // Estratégia padrão

    fun setSortStrategy(strategy: SortStrategy) {
        sortStrategy = strategy
    }

    fun showSortedPokemonList() {
        val sortedList = sortStrategy.sort(pokemonList)
        view.displayPokemonList(sortedList)
    }
}

```

## Conclusão🔆

A aplicação desses padrões (MVC, Factory, Strategy e Observer) não só melhoram a qualidade dos códigos, mas também tornariam o projeto da Pokédex mais modular, organizado e pronto para futuras expansões. Ao incorporar essas melhorias, meu projeto poderia alcançar:

* Uma organização sólida que separaria responsabilidades e facilitaria a manutenção.
* Teria facilidade para expandir o projeto, assim podendo adicionar novas funcionalidades de forma simples.
* A experiência de usuário seria amplamente aprimorada, com opções de ordenação e interface visualmente separada da lógica.
* O design do aplicativo ficaria agradável, intuitivo e atrativo para fãs da franquia e até para pessoas que teriam interesse de conhecer os Pokemons.
  
Essas mudanças, além de proporcionarem uma base sólida para possíveis novos desenvolvimentos, fariam com que meu projeto seguisse as boas práticas de desenvolvimento e manteria se escalável e fácil de entender. A implementação desses padrões tornaria minha Pokédex mais robusta e versátil, demonstrando como pequenas melhorias estruturais podem transformar um projeto em algo mais profissional e eficiente.

