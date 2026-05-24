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

## Setup: Create the Project

This lesson uses a fresh Expo app. Open a terminal and run:

```bash
npx create-expo-app --template blank LearnNavigationApp
cd LearnNavigationApp
npx expo start
```

Open the emulator or scan the QR code with Expo Go. Confirm the default screen loads.

Inside the project, create the following folder structure:

```
LearnNavigationApp/
  screens/
    HomeScreen.js
    SettingsScreen.js
  components/
    Header.js
  App.js
```

Create `components/Header.js` first. This is a simple reusable text component that will be used across all screens to display a heading:

```jsx
import { Text, StyleSheet } from 'react-native';

function Header({ children }) {
  return <Text style={styles.headerText}>{children}</Text>;
}

export default Header;

const styles = StyleSheet.create({
  headerText: {
    fontSize: 24,
    fontWeight: '700',
    marginBottom: 20,
  },
});
```

Create `screens/HomeScreen.js`:

```jsx
import { StyleSheet, Text, View } from 'react-native';
import Header from '../components/Header';

function HomeScreen() {
  return (
    <View style={styles.container}>
      <Header>Home</Header>
      <Text>Welcome to the Home screen!</Text>
    </View>
  );
}

export default HomeScreen;

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});
```

Create `screens/SettingsScreen.js` using the same structure, replacing `Home` with `Settings` and updating the welcome text.

---

## The Problem: Navigating Without a Library

Before adding React Navigation, look at what it takes to switch between screens manually.

Update `App.js` to import both screens and toggle between them with a state variable:

```jsx
import { useState } from 'react';
import { Button, StyleSheet, View } from 'react-native';
import HomeScreen from './screens/HomeScreen';
import SettingsScreen from './screens/SettingsScreen';

export default function App() {
  const [currentScreen, setCurrentScreen] = useState('Home');

  let content;
  if (currentScreen === 'Home') {
    content = <HomeScreen />;
  } else if (currentScreen === 'Settings') {
    content = <SettingsScreen />;
  }

  return (
    <View style={styles.container}>
      <Button title="Home" onPress={() => setCurrentScreen('Home')} />
      <Button title="Settings" onPress={() => setCurrentScreen('Settings')} />
      {content}
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    marginTop: 50,
  },
});
```

**Device check:** tapping the buttons switches between the two screens.

This approach works but has two serious limitations: there is no "back" functionality, and adding more screens means more `if/else` branches and more buttons. For a real app with ten or more screens, this becomes unmanageable.

This is why React Navigation exists.

---

## Part 1: Tab Navigation

### Step 1: Install React Navigation

Install the core package and its required dependencies:

```bash
npm install @react-navigation/native
npx expo install react-native-screens react-native-safe-area-context
```

`react-native-safe-area-context` was already installed in Lesson 2.15, but the command is safe to run again. Expo will confirm the correct version is in place.

### Step 2: Install the Bottom Tabs package

```bash
npm install @react-navigation/bottom-tabs
```

### Step 3: Set up the navigator

Update `App.js` to replace the manual switcher with a proper tab navigator:

```jsx
import { NavigationContainer } from '@react-navigation/native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import HomeScreen from './screens/HomeScreen';
import SettingsScreen from './screens/SettingsScreen';

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
- The `name` prop is a unique identifier used for navigation. It also becomes the header title and tab label by default, so choose it thoughtfully.

> **Common mistake:** Passing a JSX element as the `component` prop, for example `component={<HomeScreen />}`. The navigator expects a component reference (`component={HomeScreen}`), not a rendered element. Passing JSX causes React Navigation to create a new component type on every render, leading to unexpected re-mounts and state loss.

### Step 4: Add a third screen

Create `screens/ExploreScreen.js` with the same structure as `HomeScreen`, using `Explore` as the heading.

---

## Activity 1: Register the Explore Screen

Register `ExploreScreen` as a third tab between `Home` and `Settings`.

**Hints:**
1. Import `ExploreScreen` from `./screens/ExploreScreen`
2. Add a `Tab.Screen` inside `Tab.Navigator` with `name="Explore"` and `component={ExploreScreen}`
3. Order matters: tabs appear in the order they are registered

<details>
<summary>Reference solution</summary>

```jsx
import { NavigationContainer } from '@react-navigation/native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import HomeScreen from './screens/HomeScreen';
import ExploreScreen from './screens/ExploreScreen';
import SettingsScreen from './screens/SettingsScreen';

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

### Step 5: Create the HotDeals screen

Create `screens/HotDealsScreen.js`:

```jsx
import { StyleSheet, Text, View } from 'react-native';
import Header from '../components/Header';

function HotDealsScreen() {
  return (
    <View style={styles.container}>
      <Header>Hot Deals</Header>
      <Text>Welcome to the Hot Deals screen!</Text>
    </View>
  );
}

export default HotDealsScreen;

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});
```

Register it as a tab with `name="HotDeals"`. Note that the tab label and header title will show "HotDeals" by default, which is not ideal.

### Step 6: Customise screen titles and tab labels

When the screen identifier name and the desired display title differ, use the `options` prop on `Tab.Screen`:

```jsx
<Tab.Screen
  name="HotDeals"
  component={HotDealsScreen}
  options={{
    headerTitle: '🔥 Hot Deals!',
    tabBarLabel: 'Hot Deals!',
  }}
/>
```

`headerTitle` controls the text in the top header bar. `tabBarLabel` controls the label under the tab icon at the bottom.

### Step 7: Set a default screen

The first registered screen is shown on launch. To change the default without reordering tabs, use `initialRouteName` on `Tab.Navigator`:

```jsx
<Tab.Navigator initialRouteName="Explore">
```

### Step 8: Style the navigator

Use `screenOptions` on `Tab.Navigator` to apply styles to all screens at once. Add a brand colour to the header and the active tab:

```jsx
<Tab.Navigator
  initialRouteName="Explore"
  screenOptions={{
    headerStyle: { backgroundColor: '#e8590c' },
    headerTintColor: '#fff',
    tabBarActiveTintColor: '#e8590c',
  }}
>
```

`headerStyle` sets the background colour of the top header. `headerTintColor` sets the colour of the header title text. `tabBarActiveTintColor` sets the colour of the active tab icon and label.

**Device check:** the header has an orange background, and the active tab appears orange.

### Step 9: Add tab icons

Expo includes a large icon library. Import `Ionicons` from `@expo/vector-icons`:

```jsx
import { Ionicons } from '@expo/vector-icons';
```

Use the `tabBarIcon` option to set an icon for the `Home` tab. The navigator provides `color` and `size` so the icon matches the active/inactive colour and the tab bar's icon size automatically:

```jsx
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

## Part 2: Drawer Navigation

A drawer navigator presents screens in a slide-in side panel rather than tabs at the bottom. The configuration pattern is nearly identical to the tab navigator.

### Step 1: Install the drawer packages

```bash
npm install @react-navigation/drawer
npx expo install react-native-gesture-handler react-native-reanimated
```

Two additional packages are required:

- `react-native-gesture-handler` enables swipe gestures so users can open and close the drawer with a swipe
- `react-native-reanimated` powers the smooth slide-in animation without blocking the JavaScript thread

Kill the development server and restart it after installation. These are native modules and require a fresh build to load correctly.

### Step 2: Create a DrawerApp component

To keep the project self-contained, move the tab navigator into its own component and add a new drawer component alongside it. This lets you switch between the two in `App.js` to compare them.

Create a `BottomTabsApp` component by wrapping the existing tab navigator code:

```jsx
import { NavigationContainer } from '@react-navigation/native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { Ionicons } from '@expo/vector-icons';
import HomeScreen from './screens/HomeScreen';
import ExploreScreen from './screens/ExploreScreen';
import HotDealsScreen from './screens/HotDealsScreen';
import SettingsScreen from './screens/SettingsScreen';

const Tab = createBottomTabNavigator();

function BottomTabsApp() {
  return (
    <Tab.Navigator
      initialRouteName="Explore"
      screenOptions={{
        headerStyle: { backgroundColor: '#e8590c' },
        headerTintColor: '#fff',
        tabBarActiveTintColor: '#e8590c',
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
          headerTitle: '🔥 Hot Deals!',
          tabBarLabel: 'Hot Deals!',
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
      <BottomTabsApp />
    </NavigationContainer>
  );
}
```

### Step 3: Build the DrawerApp

Now create a `DrawerApp` component in the same file:

```jsx
import { createDrawerNavigator } from '@react-navigation/drawer';

const Drawer = createDrawerNavigator();

function DrawerApp() {
  return (
    <Drawer.Navigator
      screenOptions={{
        headerStyle: { backgroundColor: '#e8590c' },
        headerTintColor: '#fff',
        drawerActiveBackgroundColor: '#e8590c',
        drawerActiveTintColor: '#fff',
      }}
    >
      <Drawer.Screen name="Home" component={HomeScreen} />
      <Drawer.Screen name="Explore" component={ExploreScreen} />
      <Drawer.Screen
        name="HotDeals"
        component={HotDealsScreen}
        options={{
          headerTitle: '🔥 Hot Deals!',
          drawerLabel: 'Hot Deals!',
        }}
      />
      <Drawer.Screen name="Settings" component={SettingsScreen} />
    </Drawer.Navigator>
  );
}
```

Switch `App.js` to render `DrawerApp` inside `NavigationContainer`:

```jsx
export default function App() {
  return (
    <NavigationContainer>
      <DrawerApp />
    </NavigationContainer>
  );
}
```

**Device check:** swipe from the left edge of the screen to open the drawer. Tapping a menu item navigates to that screen. The active menu item is highlighted in orange.

Notice that the configuration API is nearly identical to the tab navigator. `screenOptions` applies to all screens; individual `options` props on each `Drawer.Screen` override per-screen settings. The only difference is the drawer-specific option names: `drawerActiveBackgroundColor`, `drawerActiveTintColor`, and `drawerLabel` instead of their tab equivalents.

### Step 4: Add drawer icons

The `drawerIcon` option works the same way as `tabBarIcon`:

```jsx
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

## Part 3: Stack Navigation

The stack navigator is different in character from the tab and drawer navigators. Rather than presenting a flat list of peer screens, it manages a history: navigating to a screen pushes it onto a stack, and pressing back pops it off. This is how most detail screens in mobile apps work.

### Step 1: Install the stack package

```bash
npm install @react-navigation/native-stack
```

There are two stack navigator packages: `@react-navigation/stack` (JavaScript-based, more customisable) and `@react-navigation/native-stack` (uses native platform navigation components, more performant). Use `native-stack` unless you have a specific reason not to.

### Step 2: Create a menu screen

Create `screens/MenuScreen.js`. This screen will serve as the starting point for stack navigation, since the stack navigator does not have a built-in tab bar or drawer to browse screens from:

```jsx
import { Button, StyleSheet, View } from 'react-native';
import Header from '../components/Header';

function MenuScreen() {
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

export default MenuScreen;

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
  buttonsContainer: {
    gap: 5,
  },
});
```

### Step 3: Build the StackApp

Create a `StackApp` component in `App.js`:

```jsx
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import MenuScreen from './screens/MenuScreen';

const Stack = createNativeStackNavigator();

function StackApp() {
  return (
    <Stack.Navigator
      screenOptions={{
        headerStyle: { backgroundColor: '#e8590c' },
        headerTintColor: '#fff',
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

Switch `App.js` to render `StackApp`:

```jsx
export default function App() {
  return (
    <NavigationContainer>
      <StackApp />
    </NavigationContainer>
  );
}
```

**Device check:** the menu screen appears with four buttons. The buttons do not navigate yet.

### Step 4: Wire up navigation with the `navigation` prop

React Navigation automatically provides a `navigation` prop to every registered screen component. Call `navigation.navigate("ScreenName")` to move to a different screen.

Update `MenuScreen` to accept and use the `navigation` prop:

```jsx
function MenuScreen({ navigation }) {
  return (
    <View style={styles.container}>
      <Header>Menu</Header>
      <View style={styles.buttonsContainer}>
        <Button title="Home" onPress={() => navigation.navigate('Home')} />
        <Button title="Explore" onPress={() => navigation.navigate('Explore')} />
        <Button title="Hot Deals" onPress={() => navigation.navigate('HotDeals')} />
        <Button title="Settings" onPress={() => navigation.navigate('Settings')} />
      </View>
    </View>
  );
}
```

**Device check:** tapping the buttons navigates to the correct screens. The header shows a back arrow that returns to the previous screen in the stack.

### Step 5: The `useNavigation` hook

The `navigation` prop is only available to components that are directly registered as screens. If you move the button list into a child component, `navigation` will be `undefined` in that child.

Move the buttons into a separate `MenuOptions` component to demonstrate this:

```jsx
function MenuOptions({ navigation }) {
  return (
    <View style={styles.buttonsContainer}>
      <Button title="Home" onPress={() => navigation.navigate('Home')} />
      <Button title="Explore" onPress={() => navigation.navigate('Explore')} />
      <Button title="Hot Deals" onPress={() => navigation.navigate('HotDeals')} />
      <Button title="Settings" onPress={() => navigation.navigate('Settings')} />
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
import { useNavigation } from '@react-navigation/native';

function MenuOptions() {
  const navigation = useNavigation();

  return (
    <View style={styles.buttonsContainer}>
      <Button title="Home" onPress={() => navigation.navigate('Home')} />
      <Button title="Explore" onPress={() => navigation.navigate('Explore')} />
      <Button title="Hot Deals" onPress={() => navigation.navigate('HotDeals')} />
      <Button title="Settings" onPress={() => navigation.navigate('Settings')} />
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

### Step 6: Passing data between screens with route parameters

Create `screens/ProductDetailScreen.js`:

```jsx
import { StyleSheet, Text, View } from 'react-native';
import Header from '../components/Header';

function ProductDetailScreen() {
  return (
    <View style={styles.container}>
      <Header>Product Detail</Header>
      <Text>Welcome to the Product Detail screen!</Text>
    </View>
  );
}

export default ProductDetailScreen;

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});
```

Register it in `StackApp`:

```jsx
<Stack.Screen name="ProductDetail" component={ProductDetailScreen} />
```

Update `HotDealsScreen` to show product buttons that navigate to `ProductDetailScreen` with data attached. To use the `navigation` prop here, either accept it as a prop (since `HotDealsScreen` is a registered screen) or use `useNavigation`:

```jsx
import { Button, StyleSheet, View } from 'react-native';
import Header from '../components/Header';

function HotDealsScreen({ navigation }) {
  return (
    <View style={styles.container}>
      <Header>Hot Deals</Header>
      <Button
        title="Apple iPad @ $299"
        onPress={() =>
          navigation.navigate('ProductDetail', {
            product: 'Apple iPad',
            id: 123,
            price: 299,
            fromScreen: 'HotDeals',
          })
        }
      />
      <Button
        title="Apple iPhone @ $999"
        onPress={() =>
          navigation.navigate('ProductDetail', {
            product: 'Apple iPhone',
            id: 124,
            price: 999,
            fromScreen: 'HotDeals',
          })
        }
      />
    </View>
  );
}
```

Now update `ProductDetailScreen` to read the params using the `useRoute` hook:

```jsx
import { StyleSheet, Text, View } from 'react-native';
import { useRoute } from '@react-navigation/native';
import Header from '../components/Header';

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

### Step 7: Set the header title dynamically

The header title on `ProductDetailScreen` currently shows "ProductDetail". To display the product name instead, use the `options` callback on `Stack.Screen`. The callback receives `{ route }`, which contains the params passed during navigation:

```jsx
<Stack.Screen
  name="ProductDetail"
  component={ProductDetailScreen}
  options={({ route }) => ({
    title: `Product Detail: ${route.params?.product ?? ''}`,
  })}
/>
```

**Device check:** the header now shows the product name, for example "Product Detail: Apple iPad".

> **When to use `useLayoutEffect` instead:** If the title depends on data fetched inside the screen after it loads (not on params passed during navigation), use `navigation.setOptions` inside `useLayoutEffect` in the screen component. `useLayoutEffect` runs synchronously before the screen is painted, preventing a visible title flash. For most cases where params are known at navigation time, the `Stack.Screen options` approach above is simpler and preferred.

---

## Part 4: State Persistence Across Navigators

This section demonstrates a behaviour that commonly surprises developers. Follow along and observe what happens.

### Stack Navigator: state is destroyed on back

Add a search input to `HomeScreen`. First, update `HomeScreen.js` to accept a `navigation` prop and add a text input:

```jsx
import { StyleSheet, Text, TextInput, View } from 'react-native';
import { useState } from 'react';
import Header from '../components/Header';

function HomeScreen({ navigation }) {
  const [searchQuery, setSearchQuery] = useState('');

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

export default HomeScreen;

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
  welcomeText: {
    fontSize: 20,
    marginBottom: 20,
  },
  searchInput: {
    height: 40,
    borderColor: '#ddd',
    borderWidth: 1,
    borderRadius: 5,
    paddingHorizontal: 10,
    marginBottom: 20,
    width: '80%',
  },
});
```

Make sure `App.js` is using `StackApp`. Now:

1. Navigate: Menu > Home
2. Type something in the search field
3. Tap the back button to return to Menu
4. Navigate to Home again

The typed text is gone. When the back button pops `HomeScreen` off the stack, React Navigation destroys the component instance along with its state.

### Tab and Drawer Navigators: state persists

Switch `App.js` to use `BottomTabsApp` and repeat the same steps:

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
import { useFocusEffect } from '@react-navigation/native';
import { useCallback, useState } from 'react';

function HomeScreen() {
  const [searchQuery, setSearchQuery] = useState('');

  useFocusEffect(
    useCallback(() => {
      setSearchQuery('');
    }, [])
  );

  // ... rest of component
}
```

The callback must be wrapped in `useCallback` with an empty dependency array. Without it, a new function reference is created on every render, and `useFocusEffect` would run on every render instead of only when the screen gains focus.

**Device check:** switch between `StackApp` and `BottomTabsApp` in `App.js`. In both cases, navigating away from Home and returning should now clear the search field.

---

## Part 5: Nesting Navigators

Production apps commonly combine navigators. A typical pattern is to nest a bottom tab navigator inside a stack navigator, so that detail screens (like `ProductDetailScreen`) can be pushed on top of any tab, and the back button returns the user to whichever tab they came from.

Create a `NestedApp` component in `App.js`:

```jsx
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';

const Stack = createNativeStackNavigator();
const Tab = createBottomTabNavigator();

function BottomTabsApp() {
  return (
    <Tab.Navigator
      screenOptions={{
        headerStyle: { backgroundColor: '#e8590c' },
        headerTintColor: '#fff',
        tabBarActiveTintColor: '#e8590c',
      }}
    >
      <Tab.Screen name="Home" component={HomeScreen} />
      <Tab.Screen name="Explore" component={ExploreScreen} />
      <Tab.Screen
        name="HotDeals"
        component={HotDealsScreen}
        options={{ headerTitle: '🔥 Hot Deals!', tabBarLabel: 'Hot Deals!' }}
      />
      <Tab.Screen name="Settings" component={SettingsScreen} />
    </Tab.Navigator>
  );
}

function NestedApp() {
  return (
    <Stack.Navigator
      screenOptions={{
        headerStyle: { backgroundColor: '#e8590c' },
        headerTintColor: '#fff',
      }}
    >
      <Stack.Screen
        name="BottomTabs"
        component={BottomTabsApp}
        options={{ headerShown: false }}
      />
      <Stack.Screen
        name="ProductDetail"
        component={ProductDetailScreen}
        options={({ route }) => ({
          title: `Product Detail: ${route.params?.product ?? ''}`,
          headerBackTitle: route.params?.fromScreen ?? 'Back',
        })}
      />
    </Stack.Navigator>
  );
}

export default function App() {
  return (
    <NavigationContainer>
      <NestedApp />
    </NavigationContainer>
  );
}
```

Key points:

- `headerShown: false` on the `BottomTabs` screen hides the outer stack's header so only the tab navigator's header is visible. Without this, two headers would appear stacked.
- `ProductDetailScreen` is registered at the outer stack level so it can be reached from any tab via `navigation.navigate('ProductDetail', params)`.
- `headerBackTitle` (iOS only) customises the label shown on the back arrow, for example "Hot Deals" instead of the generic "Back".

**Device check:** navigating to a product from the Hot Deals tab pushes `ProductDetailScreen` on top of the tabs. The tab bar disappears. Pressing back returns to the Hot Deals tab.

---

## Bonus Challenges

Work on as many as you can. They are listed in order of difficulty. No solutions are provided.

### Challenge 1: Custom tab bar label with emoji

For the HotDeals tab, set `tabBarLabel` to an emoji-prefixed string (for example, "🔥 Deals"). Set `headerTitle` separately to a different string. Observe that the two can be controlled independently.

### Challenge 2: `headerBackTitle`

In the nested navigator, update the `HotDealsScreen` to pass `fromScreen: 'Hot Deals'` in the params when navigating to `ProductDetailScreen`. Update the `ProductDetailScreen` stack screen options to use `route.params?.fromScreen ?? 'Back'` as the `headerBackTitle`. Verify on a physical iOS device or iOS simulator that the back button shows the correct label.

### Challenge 3: `useIsFocused`

Import `useIsFocused` from `@react-navigation/native`. In any screen component, use it to conditionally render `null` when the screen is not focused:

```jsx
const isFocused = useIsFocused();
if (!isFocused) return null;
```

Test this in both the tab navigator and the stack navigator. Describe in your own words how the visible behaviour differs between the two navigator types, and explain why.

### Challenge 4: Nested drawer inside tabs

Replace one of the tabs in `BottomTabsApp` with a `DrawerApp` as its component. What happens when you tap that tab? What are the usability problems with this pattern? When might nesting a drawer inside a tab actually be appropriate?

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
