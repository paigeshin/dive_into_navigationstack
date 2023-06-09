# dive_into_navigationstack

1. You need to pass `hashable` to link [NavigationLink(value: ...)] and attach navigation destination modifier to receive the value and route  
2. Or you can create global `Path` Object which is also hashable and assign it to NavigationStack and attach navigation destination modifier to receive the path and route 
3. Or you can use `NavigationPath` and assign it to NavigationStack. It behaves like Generic which can take any data type.

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


### Path with DataType 

- append => go to that page 
- removeAll => popToRoot 
- assign => go to all the links till the end 

```swift

import SwiftUI

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
    
    @State private var path: [Game] = []
    
    var body: some View {
        NavigationStack(path: self.$path) {
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
                    Button("Add Games") {
//                        self.path.append(self.games.first!)
                        self.path = self.games
                    }
                }
                
            } //: LIST
            .navigationTitle("Gaming")
            .navigationDestination(for: Platform.self) { platform in
                ZStack {
                    platform.color.ignoresSafeArea()
                    Label(platform.name, systemImage: platform.imageName)
                        .font(.largeTitle)
                        .fontWeight(.bold)
                }
            }
            .navigationDestination(for: Game.self) { game in
                Text("\(game.name) - \(game.rating)")
                    .font(.largeTitle.bold())
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

### NavigationPath (NavigationPath())

```swift
//
//  ContentView.swift
//  ProgrammaticNavigation
//
//  Created by paige shin on 2023/05/11.
//

import SwiftUI

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
    
    @State private var path = NavigationPath()
    
    var body: some View {
        NavigationStack(path: self.$path) {
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
                            Text(game.name)
                        }
                    }
                }
                
            } //: LIST
            .navigationTitle("Gaming")
            .navigationDestination(for: Platform.self) { platform in
                ZStack {
                    platform.color.ignoresSafeArea()
                    
                    VStack {
                        Label(platform.name, systemImage: platform.imageName)
                            .font(.largeTitle)
                            .fontWeight(.bold)
                        List {
                            ForEach(self.games, id: \.name) { game in
                                NavigationLink(value: game) {
                                    Text(game.name)
                                }
                            }
                        }
                    }
            
                }
            }
            .navigationDestination(for: Game.self) { game in
                VStack(spacing: 20) {
                    Text("\(game.name) - \(game.rating)")
                        .font(.largeTitle.bold())
                    
                    Button("Recommended Game") {
                        self.path.append(self.games.randomElement()!)
                    }
                    
                    Button("Go to another platform") {
                        self.path.append(self.platforms.randomElement()!)
                    }
                    
                    Button("Go Home") {
                        self.path.removeLast()
//                        self.path.removeLast(self.path.count)
//                        self.path.removeLast(2)
                    }
                }
                
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
