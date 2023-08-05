# rep

I tried to make two patterns based on this,
https://qiita.com/nownaka/items/01d28ffc638727ab3fa8

Is it possible?

Or is it not this way?

```swift

struct ContentView: View {

@Environment(\.managedObjectContext) private var viewContext

@FetchRequest(sortDescriptors: [NSSortDescriptor(keyPath: \Pokemon.timestamp, ascending: true)], animation: .default) private var pokemons: FetchedResults<Pokemon>

@FetchRequest(sortDescriptors: [NSSortDescriptor(keyPath: \Pokemon.timestamp, ascending: true)], animation: .default) private var boxes: FetchedResults<Box>

var box = Box()
var pokemon = Pokemon()

@State var pokemonNumber = 0
    
@State var boxNo = 1


init(pokemonNumber: Int) {
    self.pokemonNumber = Int(box.boxNo)
}


var body: some View {

Button(action: {saveCoreData()}) {Text("New")}
Button(action: {editCoreData()}) {Text("Update")}
Button(action: {deleteCoreData()}) {Text("Escape")}

Text("\(pokemonNumber)")

}


func saveCoreData() {
            let newPokemon = Pokemon(context: viewContext)
            newPokemon.id = UUID()
            newPokemon.name = pokemon.name
            newPokemon.pokedexNo = Int64(pokemon.pokedexNo)
            newPokemon.boxNo = box //ポケモンとボックスの紐付け
            do {
                try viewContext.save()
            } catch {
                let nsError = error as NSError
                fatalError("Unresolved error \(nsError), \(nsError.userInfo)")
            }
    }
    func editCoreData() { //保存ボタン押下時に実行

// 名前

        do {
            pokemon.boxNo = boxNo //変更先のボックスを代入
            try? viewContext.save() //保存（更新）処理
        }
    }
    func deleteCoreData() { //逃がすボタンを押下時に実行
        viewContext.delete(pokemon) //削除処理
    }


}

```

```swift

struct ContentView: View {

@Environment(\.managedObjectContext) private var viewContext

@FetchRequest(sortDescriptors: [NSSortDescriptor(keyPath: \Pokemon.timestamp, ascending: true)], animation: .default) private var pokemons: FetchedResults<Pokemon>

@FetchRequest(sortDescriptors: [NSSortDescriptor(keyPath: \Pokemon.timestamp, ascending: true)], animation: .default) private var boxs: FetchedResults<Box>

var box = Box()
var pokemon = Pokemon()
var pokemo: PokemonModel

@State var pokemonNumber = 0
    
@State var boxNo: Box?

// Variable 'self.pokemo' used before being initialized

init(pokemonNumber: Int){
    self.pokemonNumber = Int(box.boxNo)
}


var body: some View {

Button(action: {saveCoreData()}) {Text("新規")}
Button(action: {editCoreData()}) {Text("更新")}
Button(action: {deleteCoreData()}) {Text("逃がす")}

Text("\(pokemonNumber)")

}


func saveCoreData() {
            let newPokemon = Pokemon(context: viewContext)
            newPokemon.id = UUID()
            newPokemon.name = pokemo.name
            newPokemon.pokedexNo = Int64(pokemo.no)
            newPokemon.boxNo = box //ポケモンとボックスの紐付け
            do {
                try viewContext.save()
            } catch {
                let nsError = error as NSError
                fatalError("Unresolved error \(nsError), \(nsError.userInfo)")
            }
    }
    func editCoreData() { //保存ボタン押下時に実行
        do {
            pokemon.boxNo = boxNo //変更先のボックスを代入
            try? viewContext.save() //保存（更新）処理
        }
    }
    func deleteCoreData() { //逃がすボタンを押下時に実行
        viewContext.delete(pokemon) //削除処理
    }


}

class PokemonModel: Identifiable {

var name: String
var no: Int

init(name: String, no: Int){
    self.name = "テキスト"
    self.no = 1
}

}

```
