# Lesson 2.17: Application Flow Control with Navigation Frameworks

## Overview

- **Duration:** ~2 hours (hands-on lab)
- **Prerequisites:** Lesson 2.15 and 2.16 (Expo environment set up, Expo Go working on your device or emulator, familiarity with React Native core components and Flexbox)

## Learning Objectives

By the end of this lesson, you will be able to:

1. **Explain** why dedicated navigation libraries are needed in React Native and how React Navigation compares to React Router in web apps
2. **Implement** tab, drawer, and stack navigation using React Navigation, registering screens with `NavigationContainer` and the appropriate navigator
3. **Pass** data between screens using route parameters and access the navigation API from any component using the `useNavigation` hook

---

## Setup: Create the Project (10 minutes)

This lesson uses a fresh Expo app. Open a terminal and run:

```bash
npx create-expo-app --template blank learn-navigation-app
cd learn-navigation-app
npx expo start
```

Open the emulator or scan the QR code with Expo Go. Confirm the default screen loads.

As in Lesson 2.16, set up the linter now before writing any code:

```bash
npx expo lint
```

> If this is the first time linting has run in this project, the CLI will offer to install and create an ESLint config - accept the default. Keep this command handy throughout the lesson: this lab involves creating many new files and wiring up props between them, and running `npx expo lint` after each step catches typos and missing imports before they show up as a red error screen on the device.

Inside the project, create the following folder structure:

```
learn-navigation-app/
  screens/
    HomeScreen.js
    SettingsScreen.js
  components/
    Header.js
  styles/
    colors.js
  App.js
```

As in Lesson 2.16, keep the brand color in one place rather than repeating it throughout the app. This lesson configures the same orange header across every navigator, so the payoff shows up quickly. Create `styles/colors.js`:

```js
// styles/colors.js
export const Colors = {
  PRIMARY: "#e8590c",
  WHITE: "#fff",
};
```

Create `components/Header.js` next. This is a simple reusable text component that will be used across all screens to display a heading:

```jsx
// components/Header.js
import { Text, StyleSheet } from "react-native";

function Header({ children }) {
  return <Text style={styles.headerText}>{children}</Text>;
}

export default Header;

const styles = StyleSheet.create({
  headerText: {
    fontSize: 24,
    fontWeight: "700",
    marginBottom: 20,
  },
});
```

Create `screens/HomeScreen.js`:

```jsx
// screens/HomeScreen.js
import { StyleSheet, Text, View } from "react-native";
import Header from "../components/Header";
import { Colors } from "../styles/colors";

export default function HomeScreen() {
  return (
    <View style={styles.container}>
      <Header>Home</Header>
      <Text>Welcome to the Home screen!</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: Colors.WHITE,
    alignItems: "center",
    justifyContent: "center",
  },
});
```

Create `screens/SettingsScreen.js` using the same structure, replacing `Home` with `Settings` and updating the welcome text.

---

## The Problem: Navigating Without a Library (5 minutes, instructor demo)

Before adding React Navigation, look at what it takes to switch between screens manually.

Update `App.js` to import both screens and toggle between them with a state variable:

```jsx
// App.js
import { useState } from "react";
import { Button, StyleSheet, View } from "react-native";
import HomeScreen from "./screens/HomeScreen";
import SettingsScreen from "./screens/SettingsScreen";

export default function App() {
  const [currentScreen, setCurrentScreen] = useState("Home");

  let content;
  if (currentScreen === "Home") {
    content = <HomeScreen />;
  } else if (currentScreen === "Settings") {
    content = <SettingsScreen />;
  }

  return (
    <View style={styles.container}>
      <Button title="Home" onPress={() => setCurrentScreen("Home")} />
      <Button title="Settings" onPress={() => setCurrentScreen("Settings")} />
      {content}
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#fff",
    marginTop: 50,
  },
});
```

**Device check:** tapping the buttons switches between the two screens.

This approach works but has two serious limitations: there is no "back" functionality, and adding more screens means more `if/else` branches and more buttons. For a real app with ten or more screens, this becomes unmanageable.

This is why React Navigation exists.

---

## Part 1: React Navigation (5 minutes)

React Navigation is a standalone library, it is not part of React Native itself. It provides three main navigator types, each suited to a different navigation pattern:

- **Bottom Tabs**, a fixed row of tabs, usually at the bottom of the screen, for switching between a handful of top-level, equally-important sections
- **Drawer**, a slide-in side panel, useful when there are more top-level sections than comfortably fit in a tab bar
- **Stack**, a history-based navigator that pushes and pops screens, used for drill-down flows such as a list screen leading to a detail screen

All three navigator types share the same underlying package and configuration pattern. Install the core package and its required dependencies once, they are shared across every navigator used later in this lesson:

```bash
npm install @react-navigation/native
npx expo install react-native-screens react-native-safe-area-context
```

The rest of this lesson works through each navigator type in turn, starting with Bottom Tabs.

---

## Part 2: Tab Navigation (35 minutes)

### Step 1: Install the Bottom Tabs package

```bash
npm install @react-navigation/bottom-tabs
```

### Step 2: Set up the navigator

Update `App.js` to replace the manual switcher with a proper tab navigator:

```jsx
// App.js
import { NavigationContainer } from "@react-navigation/native";
import { createBottomTabNavigator } from "@react-navigation/bottom-tabs";
import HomeScreen from "./screens/HomeScreen";
import SettingsScreen from "./screens/SettingsScreen";

const Tab = createBottomTabNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Tab.Navigator>
        <Tab.Screen name="Home" component={HomeScreen} />
        <Tab.Screen name="Settings" component={SettingsScreen} />
      </Tab.Navigator>
    </NavigationContainer>
  );
}
```

**Device check:** two tabs appear at the bottom of the screen. Tapping each one shows the correct screen, and the navigator automatically provides a header with the screen name.

Three things to note about this code:

- `NavigationContainer` is the top-level wrapper that manages the navigation tree and holds the navigation state. Every React Navigation app must use it at the root.
- `Tab` gives you two components: `Tab.Navigator` (the container that manages the tabs) and `Tab.Screen` (used to register each screen inside it).
- The `name` prop is a unique identifier used for navigation. It also becomes the header title and tab label by default.

> **Common mistake:** Passing a JSX element as the `component` prop, for example `component={<HomeScreen />}`. The navigator expects a component reference (`component={HomeScreen}`), not a rendered element. Passing JSX causes React Navigation to create a new component type on every render, leading to unexpected re-mounts and state loss.

### Step 3: Add a third screen

Create `screens/ExploreScreen.js` with the same structure as `HomeScreen`, using `Explore` as the heading.

---

## Activity 1: Register the Explore Screen

Register `ExploreScreen` as a second tab between `Home` and `Settings`.

**Hints:**

1. Import `ExploreScreen` from `./screens/ExploreScreen`
2. Add a `Tab.Screen` inside `Tab.Navigator` with `name="Explore"` and `component={ExploreScreen}`
3. Order matters: tabs appear in the order they are registered

<details>
<summary>Reference solution</summary>

```jsx
// App.js
import { NavigationContainer } from "@react-navigation/native";
import { createBottomTabNavigator } from "@react-navigation/bottom-tabs";
import HomeScreen from "./screens/HomeScreen";
import ExploreScreen from "./screens/ExploreScreen";
import SettingsScreen from "./screens/SettingsScreen";

const Tab = createBottomTabNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Tab.Navigator>
        <Tab.Screen name="Home" component={HomeScreen} />
        <Tab.Screen name="Explore" component={ExploreScreen} />
        <Tab.Screen name="Settings" component={SettingsScreen} />
      </Tab.Navigator>
    </NavigationContainer>
  );
}
```

</details>

---

### Step 4: Create the HotDeals screen

Create `screens/HotDealsScreen.js`:

```jsx
// screens/HotDealsScreen.js
import { StyleSheet, Text, View } from "react-native";
import Header from "../components/Header";
import { Colors } from "../styles/colors";

export default function HotDealsScreen() {
  return (
    <View style={styles.container}>
      <Header>Hot Deals</Header>
      <Text>Welcome to the Hot Deals screen!</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: Colors.WHITE,
    alignItems: "center",
    justifyContent: "center",
  },
});
```

Register it as a tab with `name="HotDeals"`. Note that the tab label and header title will show "HotDeals" by default, which is not ideal.

### Step 5: Customise screen titles and tab labels

When the screen identifier name and the desired display title differ, use the `options` prop on `Tab.Screen`:

```jsx
// App.js
<Tab.Screen
  name="HotDeals"
  component={HotDealsScreen}
  options={{
    headerTitle: "🔥 Hot Deals!",
    tabBarLabel: "Hot Deals!",
  }}
/>
```

`headerTitle` controls the text in the top header bar. `tabBarLabel` controls the label under the tab icon at the bottom.

> **Per-screen styling:** `options` is not limited to titles and labels. Style properties like `headerStyle` and `headerTintColor` can be set here too, which overrides the navigator's styling for this one screen only. This is useful for making a single screen stand out, for example giving the Hot Deals screen its own header colour instead of the shared brand colour applied in Step 7.

### Step 6: Set a default screen

The first registered screen is shown on launch. To change the default without reordering tabs, use `initialRouteName` on `Tab.Navigator`:

```jsx
// App.js
<Tab.Navigator initialRouteName="Explore">
```

### Step 7: Style the navigator

Use `screenOptions` on `Tab.Navigator` to apply styles to all screens at once. Add a brand colour to the header and the active tab, importing it from `styles/colors.js`:

```jsx
// App.js
import { Colors } from "./styles/colors";

<Tab.Navigator
  initialRouteName="Explore"
  screenOptions={{
    headerStyle: { backgroundColor: Colors.PRIMARY },
    headerTintColor: Colors.WHITE,
    tabBarActiveTintColor: Colors.PRIMARY,
  }}
>
```

`headerStyle` sets the background colour of the top header. `headerTintColor` sets the colour of the header title text. `tabBarActiveTintColor` sets the colour of the active tab icon and label.

This is the same `headerStyle` and `headerTintColor` used in the `options` prop in Step 5, set here on `screenOptions` instead so it applies to every screen at once rather than just one.

**Device check:** the header has an orange background, and the active tab appears orange.

### Step 8: Add tab icons

Install the Ionicons icon package:

```bash
npx expo install @react-native-vector-icons/ionicons
```

> Expo previously bundled `@expo/vector-icons` by default, but this is no longer the case as of the [move away from a bundled icon library](https://expo.dev/blog/moving-away-from-expo-vector-icons). Icon sets are now installed individually as needed.

Import `Ionicons` as the default export:

```jsx
// App.js
import Ionicons from "@react-native-vector-icons/ionicons";
```

Use the `tabBarIcon` option to set an icon for the `Home` tab. The navigator provides `color` and `size` so the icon matches the active/inactive colour and the tab bar's icon size automatically:

```jsx
// App.js
<Tab.Screen
  name="Home"
  component={HomeScreen}
  options={{
    tabBarIcon: ({ color, size }) => (
      <Ionicons name="home" size={size} color={color} />
    ),
  }}
/>
```

Browse available icon names at https://icons.expo.fyi (filter by Ionicons).

---

## Activity 2: Set Icons for All Tabs

Set a `tabBarIcon` for the `Explore`, `HotDeals`, and `Settings` tabs. Choose icons that make sense for each screen.

**Hints:**

1. Browse https://icons.expo.fyi and filter by "Ionicons" to find icon names
2. The pattern is identical to the `Home` tab: `({ color, size }) => <Ionicons name="..." size={size} color={color} />`
3. Suitable starting points: `"search"` for Explore, `"bonfire-sharp"` for Hot Deals, `"settings"` for Settings

<details>
<summary>Reference solution</summary>

```jsx
// App.js
// Explore tab
options={{
  tabBarIcon: ({ color, size }) => (
    <Ionicons name="search" size={size} color={color} />
  ),
}}

// HotDeals tab
options={{
  headerTitle: '🔥 Hot Deals!',
  tabBarLabel: 'Hot Deals!',
  tabBarIcon: ({ color, size }) => (
    <Ionicons name="bonfire-sharp" size={size} color={color} />
  ),
}}

// Settings tab
options={{
  tabBarIcon: ({ color, size }) => (
    <Ionicons name="settings" size={size} color={color} />
  ),
}}
```

</details>

---

## Part 3: Drawer Navigation (25 minutes)

A drawer navigator presents screens in a slide-in side panel rather than tabs at the bottom. The configuration pattern is nearly identical to the tab navigator.

### Step 1: Install the drawer packages

```bash
npx expo install react-native-gesture-handler react-native-reanimated react-native-worklets
npm install @react-navigation/drawer
```

Three additional packages are required:

- `react-native-gesture-handler` enables swipe gestures so users can open and close the drawer with a swipe
- `react-native-reanimated` powers the smooth slide-in animation without blocking the JavaScript thread
- `react-native-worklets` provides the worklets runtime that Reanimated 4 depends on

> **Install order matters here.** Installing `react-native-reanimated` first via `npx expo install` lets Expo pin a version compatible with your installed React Native version. If `@react-navigation/drawer` is installed first, npm may try to resolve `react-native-reanimated` to its newest version before Expo gets a chance to select the compatible one, which can produce an `ERESOLVE` peer dependency error.

Kill the development server and restart it after installation. These are native modules and require a fresh build to load correctly.

### Step 2: Create a DrawerNavigator component

To keep the project self-contained, move the tab navigator into its own component and add a new drawer component alongside it. This lets you switch between the two in `App.js` to compare them.

Create a `BottomTabsNavigator` component by wrapping the existing tab navigator code:

```jsx
// App.js
import { NavigationContainer } from "@react-navigation/native";
import { createBottomTabNavigator } from "@react-navigation/bottom-tabs";
import Ionicons from "@react-native-vector-icons/ionicons";
import HomeScreen from "./screens/HomeScreen";
import ExploreScreen from "./screens/ExploreScreen";
import HotDealsScreen from "./screens/HotDealsScreen";
import SettingsScreen from "./screens/SettingsScreen";
import { Colors } from "./styles/colors";

const Tab = createBottomTabNavigator();

function BottomTabsNavigator() {
  return (
    <Tab.Navigator
      initialRouteName="Explore"
      screenOptions={{
        headerStyle: { backgroundColor: Colors.PRIMARY },
        headerTintColor: Colors.WHITE,
        tabBarActiveTintColor: Colors.PRIMARY,
      }}
    >
      <Tab.Screen
        name="Home"
        component={HomeScreen}
        options={{
          tabBarIcon: ({ color, size }) => (
            <Ionicons name="home" size={size} color={color} />
          ),
        }}
      />
      <Tab.Screen
        name="Explore"
        component={ExploreScreen}
        options={{
          tabBarIcon: ({ color, size }) => (
            <Ionicons name="search" size={size} color={color} />
          ),
        }}
      />
      <Tab.Screen
        name="HotDeals"
        component={HotDealsScreen}
        options={{
          headerTitle: "🔥 Hot Deals!",
          tabBarLabel: "Hot Deals!",
          tabBarIcon: ({ color, size }) => (
            <Ionicons name="bonfire-sharp" size={size} color={color} />
          ),
        }}
      />
      <Tab.Screen
        name="Settings"
        component={SettingsScreen}
        options={{
          tabBarIcon: ({ color, size }) => (
            <Ionicons name="settings" size={size} color={color} />
          ),
        }}
      />
    </Tab.Navigator>
  );
}

export default function App() {
  return (
    <NavigationContainer>
      <BottomTabsNavigator />
    </NavigationContainer>
  );
}
```

### Step 3: Build the DrawerNavigator

Now create a `DrawerNavigator` component in the same file:

```jsx
// App.js
import { createDrawerNavigator } from "@react-navigation/drawer";

const Drawer = createDrawerNavigator();

function DrawerNavigator() {
  return (
    <Drawer.Navigator>
      <Drawer.Screen name="Home" component={HomeScreen} />
      <Drawer.Screen name="Explore" component={ExploreScreen} />
      <Drawer.Screen name="HotDeals" component={HotDealsScreen} />
      <Drawer.Screen name="Settings" component={SettingsScreen} />
    </Drawer.Navigator>
  );
}
```

Switch `App.js` to render `DrawerNavigator` inside `NavigationContainer`:

```jsx
// App.js
export default function App() {
  return (
    <NavigationContainer>
      <DrawerNavigator />
    </NavigationContainer>
  );
}
```

**Device check:** swipe from the left edge of the screen to open the drawer. Tapping a menu item navigates to that screen.

Notice that the configuration API is nearly identical to the tab navigator: a `Navigator` component wrapping `Screen` components, each with a `name` and `component` prop.

### Step 4: Customise screen titles and drawer labels

Just as `Tab.Screen` accepts an `options` prop, so does `Drawer.Screen`. Use it to customise the HotDeals screen, whose identifier name does not read well as a title or label:

```jsx
// App.js
<Drawer.Screen
  name="HotDeals"
  component={HotDealsScreen}
  options={{
    headerTitle: "🔥 Hot Deals!",
    drawerLabel: "Hot Deals!",
  }}
/>
```

`headerTitle` works exactly as it did for the tab navigator. `drawerLabel` is the drawer equivalent of `tabBarLabel`: it controls the text shown for this screen in the slide-in menu.

### Step 5: Style the navigator

Use `screenOptions` on `Drawer.Navigator` to style every screen at once, the same way `screenOptions` worked on `Tab.Navigator`:

```jsx
// App.js
<Drawer.Navigator
  screenOptions={{
    headerStyle: { backgroundColor: Colors.PRIMARY },
    headerTintColor: Colors.WHITE,
    drawerActiveBackgroundColor: Colors.PRIMARY,
    drawerActiveTintColor: Colors.WHITE,
  }}
>
```

`headerStyle` and `headerTintColor` are identical to the tab navigator. `drawerActiveBackgroundColor` and `drawerActiveTintColor` are the drawer equivalents of `tabBarActiveTintColor`: they style the currently selected menu item instead of the currently selected tab.

**Device check:** the header has an orange background, and the active menu item is highlighted in orange.

### Step 6: Add drawer icons

The `drawerIcon` option works the same way as `tabBarIcon`:

```jsx
// App.js
<Drawer.Screen
  name="Home"
  component={HomeScreen}
  options={{
    drawerIcon: ({ color, size }) => (
      <Ionicons name="home" size={size} color={color} />
    ),
  }}
/>
```

Add icons to all four screens.

---

## Part 4: Stack Navigation (35 minutes)

The stack navigator is different in character from the tab and drawer navigators. Rather than presenting a flat list of peer screens, it manages a history: navigating to a screen pushes it onto a stack, and pressing back pops it off. This is how most detail screens in mobile apps work.

### Step 1: Install the stack package

```bash
npm install @react-navigation/native-stack
```

There are two stack navigator packages: `@react-navigation/stack` (JavaScript-based, more customisable) and `@react-navigation/native-stack` (uses native platform navigation components, more performant). Use `native-stack` unless you have a specific reason not to.

### Step 2: Create a menu screen

Create `screens/MenuScreen.js`. This screen will serve as the starting point for stack navigation, since the stack navigator does not have a built-in tab bar or drawer to browse screens from:

```jsx
// screens/MenuScreen.js
import { Button, StyleSheet, View } from "react-native";
import Header from "../components/Header";
import { Colors } from "../styles/colors";

export default function MenuScreen() {
  return (
    <View style={styles.container}>
      <Header>Menu</Header>
      <View style={styles.buttonsContainer}>
        <Button title="Home" />
        <Button title="Explore" />
        <Button title="Hot Deals" />
        <Button title="Settings" />
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: Colors.WHITE,
    alignItems: "center",
    justifyContent: "center",
  },
  buttonsContainer: {
    gap: 5,
  },
});
```

### Step 3: Build the StackNavigator

Create a `StackNavigator` component in `App.js`:

```jsx
// App.js
import { createNativeStackNavigator } from "@react-navigation/native-stack";
import MenuScreen from "./screens/MenuScreen";

const Stack = createNativeStackNavigator();

function StackNavigator() {
  return (
    <Stack.Navigator
      screenOptions={{
        headerStyle: { backgroundColor: Colors.PRIMARY },
        headerTintColor: Colors.WHITE,
      }}
    >
      <Stack.Screen name="Menu" component={MenuScreen} />
      <Stack.Screen name="Home" component={HomeScreen} />
      <Stack.Screen name="Explore" component={ExploreScreen} />
      <Stack.Screen name="HotDeals" component={HotDealsScreen} />
      <Stack.Screen name="Settings" component={SettingsScreen} />
    </Stack.Navigator>
  );
}
```

Switch `App.js` to render `StackNavigator`:

```jsx
// App.js
export default function App() {
  return (
    <NavigationContainer>
      <StackNavigator />
    </NavigationContainer>
  );
}
```

**Device check:** the menu screen appears with four buttons. The buttons do not navigate yet.

### Step 4: Customise a screen title

Just as with the tab and drawer navigators, `Stack.Screen` accepts an `options` prop for per-screen overrides. The HotDeals screen has the same problem here: its identifier name is not a good header title.

```jsx
// App.js
<Stack.Screen
  name="HotDeals"
  component={HotDealsScreen}
  options={{ headerTitle: "🔥 Hot Deals!" }}
/>
```

`headerTitle` works exactly as it did for the tab and drawer navigators. The stack navigator has no tab bar or drawer label to set alongside it, since it only ever shows one screen at a time.

**Device check:** navigate from Menu to HotDeals. The header now reads "🔥 Hot Deals!" instead of "HotDeals".

### Step 5: Wire up navigation with the `navigation` prop

React Navigation automatically provides a `navigation` prop to every registered screen component. Call `navigation.navigate("ScreenName")` to move to a different screen.

Update `MenuScreen` to accept and use the `navigation` prop:

```jsx
// screens/MenuScreen.js
function MenuScreen({ navigation }) {
  return (
    <View style={styles.container}>
      <Header>Menu</Header>
      <View style={styles.buttonsContainer}>
        <Button title="Home" onPress={() => navigation.navigate("Home")} />
        <Button
          title="Explore"
          onPress={() => navigation.navigate("Explore")}
        />
        <Button
          title="Hot Deals"
          onPress={() => navigation.navigate("HotDeals")}
        />
        <Button
          title="Settings"
          onPress={() => navigation.navigate("Settings")}
        />
      </View>
    </View>
  );
}
```

**Device check:** tapping the buttons navigates to the correct screens. The header shows a back arrow that returns to the previous screen in the stack.

### Step 6: The `useNavigation` hook

The `navigation` prop is only available to components that are directly registered as screens. If you move the button list into a child component, `navigation` will be `undefined` in that child.

Move the buttons into a separate `MenuOptions` component to demonstrate this:

```jsx
// screens/MenuScreen.js
function MenuOptions({ navigation }) {
  return (
    <View style={styles.buttonsContainer}>
      <Button title="Home" onPress={() => navigation.navigate("Home")} />
      <Button title="Explore" onPress={() => navigation.navigate("Explore")} />
      <Button
        title="Hot Deals"
        onPress={() => navigation.navigate("HotDeals")}
      />
      <Button
        title="Settings"
        onPress={() => navigation.navigate("Settings")}
      />
    </View>
  );
}

function MenuScreen({ navigation }) {
  return (
    <View style={styles.container}>
      <Header>Menu</Header>
      <MenuOptions navigation={navigation} />
    </View>
  );
}
```

This works, but passing `navigation` down as a prop becomes awkward in deeply nested components. React Navigation provides the `useNavigation` hook to solve this:

```jsx
// screens/MenuScreen.js
import { useNavigation } from "@react-navigation/native";

function MenuOptions() {
  const navigation = useNavigation();

  return (
    <View style={styles.buttonsContainer}>
      <Button title="Home" onPress={() => navigation.navigate("Home")} />
      <Button title="Explore" onPress={() => navigation.navigate("Explore")} />
      <Button
        title="Hot Deals"
        onPress={() => navigation.navigate("HotDeals")}
      />
      <Button
        title="Settings"
        onPress={() => navigation.navigate("Settings")}
      />
    </View>
  );
}

function MenuScreen() {
  return (
    <View style={styles.container}>
      <Header>Menu</Header>
      <MenuOptions />
    </View>
  );
}
```

`useNavigation` retrieves the navigation object from context, so it works in any component anywhere in the tree, not just registered screens.

### Step 7: Passing data between screens with route parameters

Create `screens/ProductDetailScreen.js`:

```jsx
// screens/ProductDetailScreen.js
import { StyleSheet, Text, View } from "react-native";
import Header from "../components/Header";
import { Colors } from "../styles/colors";

export default function ProductDetailScreen() {
  return (
    <View style={styles.container}>
      <Header>Product Detail</Header>
      <Text>Welcome to the Product Detail screen!</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: Colors.WHITE,
    alignItems: "center",
    justifyContent: "center",
  },
});
```

Register it in `StackNavigator`:

```jsx
// App.js
<Stack.Screen name="ProductDetail" component={ProductDetailScreen} />
```

So far, `navigation.navigate("ScreenName")` has only taken a single argument: the name of the screen to go to. `navigate` also accepts a second argument, an object of route parameters, which the destination screen can read once it has navigated there. This is how data is passed between screens.

Update `HotDealsScreen` to show product buttons that navigate to `ProductDetailScreen` with a params object attached. To use the `navigation` prop here, either accept it as a prop (since `HotDealsScreen` is a registered screen) or use `useNavigation`:

```jsx
// screens/HotDealsScreen.js
import { Button, StyleSheet, View } from "react-native";
import Header from "../components/Header";

function HotDealsScreen({ navigation }) {
  return (
    <View style={styles.container}>
      <Header>Hot Deals</Header>
      <Button
        title="Apple iPad @ $299"
        onPress={() =>
          navigation.navigate("ProductDetail", {
            product: "Apple iPad",
            id: 123,
            price: 299,
          })
        }
      />
      <Button
        title="Apple iPhone @ $999"
        onPress={() =>
          navigation.navigate("ProductDetail", {
            product: "Apple iPhone",
            id: 124,
            price: 999,
          })
        }
      />
      <Button
        title="Apple Watch @ $399"
        onPress={() =>
          navigation.navigate("ProductDetail", {
            product: "Apple Watch",
            id: 125,
            price: 399,
          })
        }
      />
    </View>
  );
}
```

Now update `ProductDetailScreen` to read the params using the `useRoute` hook:

```jsx
// screens/ProductDetailScreen.js
import { StyleSheet, Text, View } from "react-native";
import { useRoute } from "@react-navigation/native";
import Header from "../components/Header";

function ProductDetailScreen() {
  const { params } = useRoute();

  return (
    <View style={styles.container}>
      <Header>Product Detail</Header>
      <Text>Product: {params.product}</Text>
      <Text>ID: {params.id}</Text>
      <Text>Price: ${params.price}</Text>
    </View>
  );
}
```

**Device check:** tapping a product on the Hot Deals screen navigates to the detail screen and shows the correct product data.

### Step 8: Set the header title dynamically

Step 4 set a static title with a plain object. The header title on `ProductDetailScreen` needs something different: it currently shows "ProductDetail", but the title should reflect whichever product was tapped. For this, pass a function to `options` instead of an object. The callback receives `{ route }`, which contains the params passed during navigation:

```jsx
// App.js
<Stack.Screen
  name="ProductDetail"
  component={ProductDetailScreen}
  options={({ route }) => ({
    title: `Product Detail: ${route.params.product}`,
  })}
/>
```

**Device check:** the header now shows the product name, for example "Product Detail: Apple iPad".

> **When to use `useLayoutEffect` instead:** If the title depends on data fetched inside the screen after it loads (not on params passed during navigation), use `navigation.setOptions` inside `useLayoutEffect` in the screen component. `useLayoutEffect` runs synchronously before the screen is painted, preventing a visible title flash. For most cases where params are known at navigation time, the `Stack.Screen options` approach above is simpler and preferred.

---

## Part 5: State Persistence Across Navigators (15 minutes)

This section demonstrates a behaviour that commonly surprises developers. Follow along and observe what happens.

### Stack Navigator: state is destroyed on back

Add a search input to `HomeScreen`. First, update `HomeScreen.js` to accept a `navigation` prop and add a text input:

```jsx
// screens/HomeScreen.js
import { StyleSheet, Text, TextInput, View } from "react-native";
import { useState } from "react";
import Header from "../components/Header";
import { Colors } from "../styles/colors";

export default function HomeScreen({ navigation }) {
  const [searchQuery, setSearchQuery] = useState("");

  return (
    <View style={styles.container}>
      <Header>Home</Header>
      <Text style={styles.welcomeText}>Welcome to the Home screen!</Text>
      <TextInput
        style={styles.searchInput}
        placeholder="Search for products..."
        value={searchQuery}
        onChangeText={setSearchQuery}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: Colors.WHITE,
    alignItems: "center",
    justifyContent: "center",
  },
  welcomeText: {
    fontSize: 20,
    marginBottom: 20,
  },
  searchInput: {
    height: 40,
    borderColor: "#ddd",
    borderWidth: 1,
    borderRadius: 5,
    paddingHorizontal: 10,
    marginBottom: 20,
    width: "80%",
  },
});
```

Make sure `App.js` is using `StackNavigator`. Now:

1. Navigate: Menu > Home
2. Type something in the search field
3. Tap the back button to return to Menu
4. Navigate to Home again

The typed text is gone. When the back button pops `HomeScreen` off the stack, React Navigation destroys the component instance along with its state.

### Tab and Drawer Navigators: state persists

Switch `App.js` to use `BottomTabsNavigator` and repeat the same steps:

1. Go to the Home tab
2. Type something in the search field
3. Switch to another tab
4. Switch back to Home

The typed text is still there. In tab and drawer navigators, screens remain mounted in the background when you switch away. Only their visibility changes. Because the component is never unmounted, its state is preserved.

> **Why does this matter?** If you use a stack navigator for screens that users frequently navigate back and forth between, they lose any unsaved input or scroll position. Tab and drawer navigators avoid this because screens stay mounted, but this also means they use more memory. Choose your navigator type with these trade-offs in mind.

---

### Resetting state on focus with `useFocusEffect`

What if you want to clear the search field each time the user arrives at `HomeScreen`, regardless of which navigator is in use? React Navigation provides the `useFocusEffect` hook, which runs a callback whenever a screen gains focus.

Update `HomeScreen.js`:

```jsx
// screens/HomeScreen.js
import { useFocusEffect } from "@react-navigation/native";
import { useCallback, useState } from "react";

function HomeScreen() {
  const [searchQuery, setSearchQuery] = useState("");

  useFocusEffect(
    useCallback(() => {
      setSearchQuery("");
    }, []),
  );

  // ... rest of component
}
```

The callback must be wrapped in `useCallback` with an empty dependency array. Without it, a new function reference is created on every render, and `useFocusEffect` would run on every render instead of only when the screen gains focus.

**Device check:** switch between `StackNavigator` and `BottomTabsNavigator` in `App.js`. In both cases, navigating away from Home and returning should now clear the search field.

---

## Part 6: Nesting Navigators (15 minutes)

Production apps commonly combine navigators. A typical pattern is to nest a bottom tab navigator inside a stack navigator, so that detail screens (like `ProductDetailScreen`) can be pushed on top of any tab, and the back button returns the user to whichever tab they came from.

This is a good point to move navigator configuration out of `App.js` and into its own `navigators/` folder, since `App.js` is starting to hold a lot of unrelated setup. Create `navigators/BottomTabsNavigator.js`:

```jsx
// navigators/BottomTabsNavigator.js
import { createBottomTabNavigator } from "@react-navigation/bottom-tabs";
import Ionicons from "@react-native-vector-icons/ionicons";
import HomeScreen from "../screens/HomeScreen";
import ExploreScreen from "../screens/ExploreScreen";
import HotDealsScreen from "../screens/HotDealsScreen";
import SettingsScreen from "../screens/SettingsScreen";
import { Colors } from "../styles/colors";

const Tab = createBottomTabNavigator();

export default function BottomTabsNavigator() {
  return (
    <Tab.Navigator
      initialRouteName="Explore"
      screenOptions={{
        headerStyle: { backgroundColor: Colors.PRIMARY },
        headerTintColor: Colors.WHITE,
        tabBarActiveTintColor: Colors.PRIMARY,
      }}
    >
      <Tab.Screen
        name="Home"
        component={HomeScreen}
        options={{
          tabBarIcon: ({ color, size }) => (
            <Ionicons name="home" size={size} color={color} />
          ),
        }}
      />
      <Tab.Screen
        name="Explore"
        component={ExploreScreen}
        options={{
          tabBarIcon: ({ color, size }) => (
            <Ionicons name="search" size={size} color={color} />
          ),
        }}
      />
      <Tab.Screen
        name="HotDeals"
        component={HotDealsScreen}
        options={{
          headerTitle: "🔥 Hot Deals!",
          tabBarLabel: "Hot Deals!",
          tabBarIcon: ({ color, size }) => (
            <Ionicons name="bonfire-sharp" size={size} color={color} />
          ),
        }}
      />
      <Tab.Screen
        name="Settings"
        component={SettingsScreen}
        options={{
          tabBarIcon: ({ color, size }) => (
            <Ionicons name="settings" size={size} color={color} />
          ),
        }}
      />
    </Tab.Navigator>
  );
}
```

Create `navigators/NestedNavigator.js`, which nests `BottomTabsNavigator` inside a stack:

```jsx
// navigators/NestedNavigator.js
import { createNativeStackNavigator } from "@react-navigation/native-stack";
import BottomTabsNavigator from "./BottomTabsNavigator";
import ProductDetailScreen from "../screens/ProductDetailScreen";
import { Colors } from "../styles/colors";

const Stack = createNativeStackNavigator();

export default function NestedNavigator() {
  return (
    <Stack.Navigator
      screenOptions={{
        headerStyle: { backgroundColor: Colors.PRIMARY },
        headerTintColor: Colors.WHITE,
      }}
    >
      <Stack.Screen name="BottomTabs" component={BottomTabsNavigator} />
      <Stack.Screen
        name="ProductDetail"
        component={ProductDetailScreen}
        options={({ route }) => ({
          title: `Product Detail: ${route.params.product}`,
        })}
      />
    </Stack.Navigator>
  );
}
```

Update `App.js` to render `NestedNavigator`:

```jsx
// App.js
import { NavigationContainer } from "@react-navigation/native";
import NestedNavigator from "./navigators/NestedNavigator";

export default function App() {
  return (
    <NavigationContainer>
      <NestedNavigator />
    </NavigationContainer>
  );
}
```

**Device check:** two headers now appear stacked on top of each other on the Home, Explore, Hot Deals, and Settings tabs, one from `BottomTabsNavigator` and one from the outer `NestedNavigator`. Each tab is a screen registered inside `NestedNavigator`, and `NestedNavigator` gives every screen it registers a header by default, `BottomTabsNavigator` included, even though it already renders its own header internally.

Fix this by hiding the outer stack's header for the `BottomTabs` screen specifically:

```jsx
// navigators/NestedNavigator.js
<Stack.Screen
  name="BottomTabs"
  component={BottomTabsNavigator}
  options={{ headerShown: false }}
/>
```

`headerShown: false` only affects this one `Stack.Screen`; `ProductDetailScreen` is untouched and keeps its own header from the outer stack.

Key points:

- `ProductDetailScreen` is registered at the outer stack level so it can be reached from any tab via `navigation.navigate('ProductDetail', params)`.
- The back button on `ProductDetailScreen` appears automatically, the stack navigator always provides one when there is a previous screen to return to. By default it is labelled with the previous screen's title (iOS only shows this label; Android just shows an arrow).

**Device check:** only one header appears on each tab. Navigating to a product from the Hot Deals tab pushes `ProductDetailScreen` on top of the tabs. The tab bar disappears. Pressing back returns to the Hot Deals tab.

---

## Part 7: Authenticated Navigation (30 minutes, only if time permits)

> **Only if time permits.** This section adds a login flow to the app using the same navigation patterns already covered. It requires familiarity with the Context API from Lesson 2.6.

Many apps show a different set of screens depending on whether the user is logged in. The standard pattern in React Navigation is to maintain two separate navigator stacks and conditionally render one based on authentication state.

### Step 1: Create the Auth Context

Create `contexts/AuthContext.js`. This context holds the authentication state and exposes `login` and `logout` functions:

```jsx
// contexts/AuthContext.js
import { createContext, useState } from "react";

export const AuthContext = createContext();

export function AuthProvider({ children }) {
  const [isAuthenticated, setIsAuthenticated] = useState(false);

  const login = async (username, password) => {
    // In a real app, call your API here and check credentials
    setIsAuthenticated(true);
  };

  const logout = () => {
    setIsAuthenticated(false);
  };

  return (
    <AuthContext.Provider value={{ isAuthenticated, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}
```

### Step 2: Create the Login and Register screens

Create `screens/LoginScreen.js`:

```jsx
// screens/LoginScreen.js
import { useContext, useState } from "react";
import { Button, StyleSheet, Text, TextInput, View } from "react-native";
import { AuthContext } from "../contexts/AuthContext";
import { Colors } from "../styles/colors";

export default function LoginScreen({ navigation }) {
  const { login } = useContext(AuthContext);
  const [username, setUsername] = useState("");
  const [password, setPassword] = useState("");

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Login</Text>
      <TextInput
        style={styles.input}
        placeholder="Username"
        value={username}
        onChangeText={setUsername}
      />
      <TextInput
        style={styles.input}
        placeholder="Password"
        value={password}
        onChangeText={setPassword}
        secureTextEntry
      />
      <View style={styles.buttonsContainer}>
        <Button title="Login" onPress={() => login(username, password)} />
        <Button
          title="Register"
          onPress={() => navigation.navigate("Register")}
        />
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
    padding: 16,
    backgroundColor: Colors.WHITE,
  },
  title: {
    fontSize: 24,
    fontWeight: "700",
    marginBottom: 24,
  },
  input: {
    width: "70%",
    borderWidth: 1,
    borderColor: "#ccc",
    borderRadius: 4,
    padding: 8,
    marginBottom: 16,
  },
  buttonsContainer: {
    flexDirection: "row",
    gap: 10,
  },
});
```

Create `screens/RegisterScreen.js` with the same structure, replacing the Login button with a Register button that calls `navigation.goBack()` after a mock registration:

```jsx
// screens/RegisterScreen.js
import { useState } from "react";
import { Alert, Button, StyleSheet, Text, TextInput, View } from "react-native";
import { Colors } from "../styles/colors";

export default function RegisterScreen({ navigation }) {
  const [username, setUsername] = useState("");
  const [password, setPassword] = useState("");

  const handleRegister = () => {
    Alert.alert("Mock Register", "This is a mock register function.");
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Register</Text>
      <TextInput
        style={styles.input}
        placeholder="Username"
        value={username}
        onChangeText={setUsername}
      />
      <TextInput
        style={styles.input}
        placeholder="Password"
        value={password}
        onChangeText={setPassword}
        secureTextEntry
      />
      <View style={styles.buttonsContainer}>
        <Button title="Register" onPress={handleRegister} />
        <Button title="Back to Login" onPress={() => navigation.goBack()} />
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
    padding: 16,
    backgroundColor: Colors.WHITE,
  },
  title: {
    fontSize: 24,
    fontWeight: "700",
    marginBottom: 24,
  },
  input: {
    width: "70%",
    borderWidth: 1,
    borderColor: "#ccc",
    borderRadius: 4,
    padding: 8,
    marginBottom: 16,
  },
  buttonsContainer: {
    flexDirection: "row",
    gap: 10,
  },
});
```

### Step 3: Create the navigator files

Create a `navigators/` folder to keep the navigation configuration organised.

Create `navigators/AuthStackNavigator.js`. This stack is shown when the user is not logged in:

```jsx
// navigators/AuthStackNavigator.js
import { createNativeStackNavigator } from "@react-navigation/native-stack";
import LoginScreen from "../screens/LoginScreen";
import RegisterScreen from "../screens/RegisterScreen";
import { Colors } from "../styles/colors";

const Stack = createNativeStackNavigator();

export default function AuthStackNavigator() {
  return (
    <Stack.Navigator
      screenOptions={{
        headerStyle: { backgroundColor: Colors.PRIMARY },
        headerTintColor: Colors.WHITE,
        headerShown: false,
      }}
    >
      <Stack.Screen name="Login" component={LoginScreen} />
      <Stack.Screen name="Register" component={RegisterScreen} />
    </Stack.Navigator>
  );
}
```

Create `navigators/AppStackNavigator.js`. This stack is shown when the user is logged in. It wraps the `BottomTabsNavigator` from Part 6 and adds `ProductDetailScreen` at the outer level:

```jsx
// navigators/AppStackNavigator.js
import { createNativeStackNavigator } from "@react-navigation/native-stack";
import BottomTabsNavigator from "./BottomTabsNavigator";
import ProductDetailScreen from "../screens/ProductDetailScreen";
import { Colors } from "../styles/colors";

const Stack = createNativeStackNavigator();

export default function AppStackNavigator() {
  return (
    <Stack.Navigator
      screenOptions={{
        headerStyle: { backgroundColor: Colors.PRIMARY },
        headerTintColor: Colors.WHITE,
      }}
    >
      <Stack.Screen
        name="BottomTabs"
        component={BottomTabsNavigator}
        options={{ headerShown: false }}
      />
      <Stack.Screen
        name="ProductDetail"
        component={ProductDetailScreen}
        options={({ route }) => ({
          title: `Product Detail: ${route.params.product}`,
        })}
      />
    </Stack.Navigator>
  );
}
```

### Step 4: Create the NavigationApp and conditionally render the correct stack

Create `navigators/NavigationApp.js`. This component reads `isAuthenticated` from context and renders either the app stack or the auth stack:

```jsx
// navigators/NavigationApp.js
import { useContext } from "react";
import { ActivityIndicator, View } from "react-native";
import { AuthContext } from "../contexts/AuthContext";
import AppStackNavigator from "./AppStackNavigator";
import AuthStackNavigator from "./AuthStackNavigator";

export default function NavigationApp() {
  const { isAuthenticated } = useContext(AuthContext);

  if (isAuthenticated === null) {
    return (
      <View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
        <ActivityIndicator size="large" />
      </View>
    );
  }

  return isAuthenticated ? <AppStackNavigator /> : <AuthStackNavigator />;
}
```

### Step 5: Wire everything up in App.js

Update `App.js` to wrap `NavigationApp` with both `AuthProvider` and `NavigationContainer`:

```jsx
// App.js
import { NavigationContainer } from "@react-navigation/native";
import { AuthProvider } from "./contexts/AuthContext";
import NavigationApp from "./navigators/NavigationApp";

export default function App() {
  return (
    <AuthProvider>
      <NavigationContainer>
        <NavigationApp />
      </NavigationContainer>
    </AuthProvider>
  );
}
```

**Device check:** the app opens on the Login screen. Tapping Login navigates into the main app with the bottom tab navigator. There is no back button because the entire navigator stack was swapped, not pushed.

### Step 6: Add a Logout button

Add a Logout button to `SettingsScreen` so users can return to the login flow:

```jsx
// screens/SettingsScreen.js
import { useContext } from "react";
import { Button, StyleSheet, View } from "react-native";
import { AuthContext } from "../contexts/AuthContext";
import Header from "../components/Header";
import { Colors } from "../styles/colors";

export default function SettingsScreen() {
  const { logout } = useContext(AuthContext);

  return (
    <View style={styles.container}>
      <Header>Settings</Header>
      <Button title="Logout" onPress={logout} />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: Colors.WHITE,
    alignItems: "center",
    justifyContent: "center",
  },
});
```

**Device check:** tapping Logout returns to the Login screen. The navigation history is cleared because the navigator stack itself was replaced, so the user cannot press back into the app.

> Swapping the entire navigator stack on login/logout is the standard React Navigation pattern for authentication. Because `NavigationApp` conditionally returns a completely different navigator, React Navigation unmounts the old stack and mounts the new one. There is no need to manually clear navigation history.

---

## Bonus Challenges

Work on as many as you can. They are listed in order of difficulty. No solutions are provided.

### Challenge 1: Custom tab bar label with emoji

For the HotDeals tab, set `tabBarLabel` to an emoji-prefixed string (for example, "🔥 Deals"). Set `headerTitle` separately to a different string. Observe that the two can be controlled independently.

### Challenge 2: `useIsFocused`

Import `useIsFocused` from `@react-navigation/native`. In any screen component, use it to conditionally render `null` when the screen is not focused:

```jsx
const isFocused = useIsFocused();
if (!isFocused) return null;
```

Test this in both the tab navigator and the stack navigator. Describe in your own words how the visible behaviour differs between the two navigator types, and explain why.

### Challenge 3: Nested drawer inside tabs

Replace one of the tabs in `BottomTabsNavigator` with a `DrawerNavigator` as its component. What happens when you tap that tab? What are the usability problems with this pattern? When might nesting a drawer inside a tab actually be appropriate?

---

## Summary

- React Native apps need a dedicated navigation library because there is no browser history stack. React Navigation is the standard choice and works similarly to React Router, but is designed for native mobile patterns.
- The three core navigators serve different UX purposes: Bottom Tabs for flat peer navigation, Drawer for a collapsible side menu, and Stack for hierarchical drill-down flows.
- All three navigators share the same configuration API: `NavigationContainer` at the root, a `Navigator` component, and `Screen` components with `name`, `component`, and `options` props.
- The `navigation` prop is provided automatically to registered screens. The `useNavigation` hook makes it available to any component in the tree.
- State persistence differs by navigator: stack screens lose state when popped; tab and drawer screens retain state because they stay mounted.

---

## Additional Resources

- [React Navigation: Getting Started](https://reactnavigation.org/docs/hello-react-navigation)
- [React Navigation: Bottom Tab Navigator](https://reactnavigation.org/docs/bottom-tab-navigator)
- [React Navigation: Drawer Navigator](https://reactnavigation.org/docs/drawer-navigator)
- [React Navigation: Native Stack Navigator](https://reactnavigation.org/docs/native-stack-navigator)
- [Expo Vector Icons](https://icons.expo.fyi)
