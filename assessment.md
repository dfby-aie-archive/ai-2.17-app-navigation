# Assessment: Application Flow Control with Navigation Frameworks

## Overview

- **Lesson:** Application Flow Control with Navigation Frameworks / 2.17
- **Format:** 30 questions (MCQ and True/False)
- **Time:** ~30 minutes
- **Scoring:** 1 point each

## Questions

### Q1

Which component must wrap the entire navigation tree in every React Navigation app?

A - `NavigationProvider`

B - `NavigationContainer`

C - `NavigationRoot`

D - `AppNavigator`

---

### Q2 (True/False)

React Navigation and React Router solve the same problem and can be used interchangeably in both React web apps and React Native apps.

A - True

B - False

---

### Q3

What is the correct way to pass a component to a `Tab.Screen`?

A - `component={<HomeScreen />}`

B - `component="HomeScreen"`

C - `component={HomeScreen}`

D - `render={() => <HomeScreen />}`

---

### Q4

Which package provides the Bottom Tab Navigator?

A - `@react-navigation/native`

B - `@react-navigation/bottom-tabs`

C - `@react-navigation/native-stack`

D - `@react-navigation/tabs`

---

### Q5

A developer registers the following screens in a tab navigator. Which screen is shown by default on launch?

```jsx
<Tab.Navigator>
  <Tab.Screen name="Settings" component={SettingsScreen} />
  <Tab.Screen name="Home" component={HomeScreen} />
  <Tab.Screen name="Explore" component={ExploreScreen} />
</Tab.Navigator>
```

A - `HomeScreen`, because it is named "Home"

B - `SettingsScreen`, because it is registered first

C - `ExploreScreen`, because it is registered last

D - The navigator throws an error because `initialRouteName` is not set

---

### Q6

Which prop on `Tab.Navigator` sets the initial screen without changing the order of the tabs?

A - `defaultRoute`

B - `startScreen`

C - `initialRouteName`

D - `firstScreen`

---

### Q7 (True/False)

The `name` prop on `Tab.Screen` is used only as an internal identifier and has no effect on what the user sees.

A - True

B - False

---

### Q8

A developer wants the tab label to read "Hot Deals!" but the header title to read "Today's Hot Deals". Which `options` configuration achieves this?

A - `options={{ title: "Hot Deals!", header: "Today's Hot Deals" }}`

B - `options={{ tabBarLabel: "Hot Deals!", headerTitle: "Today's Hot Deals" }}`

C - `options={{ label: "Hot Deals!", title: "Today's Hot Deals" }}`

D - `options={{ tabLabel: "Hot Deals!", screenTitle: "Today's Hot Deals" }}`

---

### Q9

Which `screenOptions` property on `Tab.Navigator` changes the colour of the active tab icon and label?

A - `tabBarSelectedColor`

B - `activeColor`

C - `tabBarActiveTintColor`

D - `selectedTintColor`

---

### Q10

A developer sets `tabBarIcon` on a `Tab.Screen`. The icon function receives `{ color, size }` from the navigator. Why is it better to use these provided values rather than hardcoding the icon size and colour?

A - Hardcoded sizes cause a runtime error in React Navigation

B - The provided values ensure the icon matches the navigator's active/inactive colour and the platform's default tab bar icon size

C - The `color` value is required by `Ionicons` and cannot be set manually

D - Using the provided values prevents the icon from scaling with device resolution

---

### Q11

Which two packages must be installed as additional dependencies when setting up the Drawer Navigator?

A - `react-native-gesture-handler` and `react-native-reanimated`

B - `react-native-screens` and `react-native-safe-area-context`

C - `react-native-drawer` and `react-native-animation`

D - `expo-gesture` and `expo-animation`

---

### Q12 (True/False)

After installing `react-native-gesture-handler` and `react-native-reanimated`, the development server must be restarted before the drawer will work correctly.

A - True

B - False

---

### Q13

Which option configures the background colour of the currently active item in the Drawer Navigator?

A - `drawerActiveItemColor`

B - `drawerSelectedBackground`

C - `drawerActiveBackgroundColor`

D - `activeItemBackground`

---

### Q14

What is the key behavioural difference between the Stack Navigator and the Bottom Tab Navigator?

A - The Stack Navigator only supports two screens; the Tab Navigator supports unlimited screens

B - The Stack Navigator manages a history of screens and destroys them when popped; the Tab Navigator keeps all screens mounted simultaneously

C - The Stack Navigator does not support headers; the Tab Navigator adds one automatically

D - The Tab Navigator navigates using `navigation.push()`; the Stack Navigator uses `navigation.navigate()`

---

### Q15

A developer is starting a new React Native project and must choose between `@react-navigation/stack` and `@react-navigation/native-stack`. Which should they choose and why?

A - `@react-navigation/stack`, because it has more configuration options and is suitable for all projects

B - `@react-navigation/native-stack`, because it uses native platform navigation components and is more performant

C - `@react-navigation/stack`, because `native-stack` is not compatible with Expo

D - Either works identically; there is no practical difference between them

---

### Q16

React Navigation automatically provides a `navigation` prop to which components?

A - All components in the app

B - Only the root `App` component

C - Only components directly registered as screens with a `Stack.Screen`, `Tab.Screen`, or `Drawer.Screen`

D - Only components that import `useNavigation`

---

### Q17

A developer has a `CartButton` component nested several levels deep inside `HomeScreen`. `HomeScreen` is a registered stack screen. What is the recommended way to give `CartButton` access to the navigation object?

A - Pass `navigation` as a prop through every level of the component tree

B - Use the `useNavigation` hook inside `CartButton`

C - Register `CartButton` as a screen so it receives the `navigation` prop automatically

D - Import `navigation` directly from `@react-navigation/native` as a named export

---

### Q18

Which of the following correctly navigates to a screen named `"ProductDetail"` and passes a `productId` parameter?

A - `navigation.navigate('ProductDetail', productId: 123)`

B - `navigation.go('ProductDetail', { productId: 123 })`

C - `navigation.navigate('ProductDetail', { productId: 123 })`

D - `navigation.push('ProductDetail', productId: 123)`

---

### Q19

Inside `ProductDetailScreen`, a developer wants to read the `productId` param passed during navigation. Which hook provides access to the route params?

A - `useParams`

B - `useRoute`

C - `useNavigation`

D - `useScreenParams`

---

### Q20 (True/False)

Both `navigation` and `route` props are available to screen components in all three navigator types: Stack, Drawer, and Bottom Tabs.

A - True

B - False

---

### Q21

A developer wants the header title of `ProductDetailScreen` to show the product name, which is passed as a route param called `product`. The product name is known at navigation time. Which approach is recommended?

A - Use `useLayoutEffect` and `navigation.setOptions` inside the screen component

B - Use a `useEffect` to update the title after the screen loads

C - Use the `options` callback on `Stack.Screen`, reading `route.params.product`

D - Set `headerTitle` as a static string on `NavigationContainer`

---

### Q22

When should a developer use `useLayoutEffect` with `navigation.setOptions` to set a screen's header title, rather than the `options` callback on `Stack.Screen`?

A - When the title depends only on params passed during navigation

B - When the title depends on data fetched inside the screen after it has loaded

C - When the screen is inside a Drawer Navigator rather than a Stack Navigator

D - When the developer prefers to keep all configuration inside the screen component

---

### Q23

A learner types text into a search field on `HomeScreen` in a Stack Navigator, presses the back button to return to `MenuScreen`, then navigates to `HomeScreen` again. What happens to the typed text?

A - The text is preserved because React Navigation caches screen state between navigations

B - The text is gone because pressing back pops `HomeScreen` off the stack, destroying its component instance and state

C - The text is gone because `useState` resets automatically on any navigation event

D - The text is preserved because the stack navigator keeps all screens mounted in the background

---

### Q24

A learner performs the same steps as Q23, but using the Bottom Tab Navigator: they type in the search field on the Home tab, switch to another tab, then switch back. What happens to the typed text?

A - The text is gone because each tab always renders a fresh component instance

B - The text is gone because `useFocusEffect` clears state automatically on tab switch

C - The text is preserved because tab screens remain mounted in the background when not visible

D - The text is preserved because `NavigationContainer` saves and restores state globally

---

### Q25 (True/False)

In a Bottom Tab Navigator, switching from one tab to another unmounts the previous screen's component.

A - True

B - False

---

### Q26

Which React Navigation hook runs a callback each time a screen gains focus?

A - `useEffect` with no dependency array

B - `useScreenFocus`

C - `useFocusEffect`

D - `useOnFocus`

---

### Q27

Why must the callback passed to `useFocusEffect` be wrapped in `useCallback`?

A - `useFocusEffect` only accepts memoised functions as a technical API requirement

B - Without `useCallback`, a new function reference is created on every render, causing the effect to fire on every render instead of only when the screen gains focus

C - `useCallback` prevents the callback from running on the initial render of the screen

D - The `useCallback` wrapper ensures the callback has access to the navigation context

---

### Q28

A developer nests a Bottom Tab Navigator inside a Native Stack Navigator. They want only the tab navigator's header to be visible, not an additional header from the outer stack. Which configuration achieves this?

A - `options={{ showHeader: false }}` on the outer `Stack.Screen`

B - `options={{ headerShown: false }}` on the outer `Stack.Screen` that renders the tab navigator

C - `screenOptions={{ headerEnabled: false }}` on the outer `Stack.Navigator`

D - Removing `NavigationContainer` from the outer stack

---

### Q29

In a nested navigator setup where a Bottom Tab Navigator is wrapped inside a Stack Navigator, at which level should `ProductDetailScreen` be registered so that it can be navigated to from any tab?

A - Inside each individual tab screen component, once per tab

B - At the inner Bottom Tab Navigator level as an additional tab

C - At the outer Stack Navigator level

D - As a standalone component outside `NavigationContainer`

---

### Q30

A developer wants to customise the iOS back button label on `ProductDetailScreen` to show the name of the originating screen. The originating screen name is passed as a param `fromScreen`. Which configuration achieves this?

A - `options={{ backLabel: route.params?.fromScreen }}`

B - `options={{ headerBackButtonLabel: route.params?.fromScreen }}`

C - `options={({ route }) => ({ headerBackTitle: route.params?.fromScreen ?? 'Back' })}`

D - `options={{ backTitle: route.params?.fromScreen }}`

---
