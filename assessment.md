# Assessment: Application Flow Control with Navigation Frameworks

## Overview

- **Lesson:** Application Flow Control with Navigation Frameworks / 2.17
- **Format:** 10 questions (MCQ and True/False)
- **Time:** ~10–15 minutes
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
