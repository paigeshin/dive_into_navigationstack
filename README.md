# dive_into_navigationstack


### Basic

```swift
struct ContentView: View {
    
    var platforms: [Platform] = [
        .init(name: "Xbox", imageName: "xbox.logo", color: .green),
        .init(name: "Playstation", imageName: "playstation.logo", color: .indigo),
        .init(name: "PC", imageName: "pc", color: .pink),
        .init(name: "Mobile", imageName: "iphone", color: .mint),
    ]
    
    var games: [Game] = [
        .init(name: "Minecraft", rating: "99"),
        .init(name: "God of War", rating: "98"),
        .init(name: "Fornite", rating: "92"),
        .init(name: "MODERN 2023", rating: "88"),
    ]
    
    var body: some View {
        NavigationStack {
            List {
                Section("Platforms") {
                    ForEach(self.platforms, id: \.name) { platform in
                        NavigationLink(value: platform) {
                            Label(platform.name, systemImage: platform.imageName)
                                .foregroundColor(platform.color)
                        }
                    } //: FOREACH
                } //: SECTION
                
                Section("Games") {
                    ForEach(self.games, id: \.name) { game in
                        NavigationLink(value: game) {
                            NavigationLink(value: game) {
                                Text(game.name)
                            }
                        }
                    } //: FOREACH
                }
                
            } //: LIST
            .navigationTitle("Gaming")
            .navigationDestination(for: Platform.self) { platform in
                ZStack {
                    platform.color.ignoresSafeArea()
                    Label(platform.name, systemImage: platform.imageName)
                        .font(.largeTitle)
                }
            }
            .navigationDestination(for: Game.self) { game in
                Text(game.name)
            }
        } //: NavigationStack

    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}

struct Platform: Hashable {
    let name: String
    let imageName: String
    let color: Color
}

struct Game: Hashable {
    let name: String
    let rating: String
}

```
