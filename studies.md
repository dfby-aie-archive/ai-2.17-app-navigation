# Pre-Reading: Lesson 2.17: Application Flow Control with Navigation Frameworks

Timebox **2–3 hours** across these resources before the lesson. You do not need to memorise everything; the goal is to build a mental model so the hands-on lab makes sense faster.

---

## 1. Why Navigation Libraries Exist

**Read (10 min)**

- [React Navigation: Hello React Navigation](https://reactnavigation.org/docs/hello-react-navigation): Read the "Installation" section and the introductory paragraphs. Focus on what problem the library solves, not the specific API details.

**Key ideas to take away:**

- React Native has no browser and no URL bar, so there is no built-in navigation history. A dedicated library is needed to manage which screen is shown and how users move between them.
- React Navigation works similarly to React Router in concept (you register screens and navigate between them by name), but it is designed for the constraints and conventions of native mobile platforms.

---

## 2. The Three Navigator Types

**Read (30 min)**

Read the official documentation for each of the three navigators used in this lesson. For each one, read the introduction and the props table, paying attention to the `screenOptions` and per-screen `options` available.

- [Bottom Tab Navigator](https://reactnavigation.org/docs/bottom-tab-navigator): Note `tabBarIcon`, `tabBarLabel`, `tabBarActiveTintColor`, and `initialRouteName`.
- [Drawer Navigator](https://reactnavigation.org/docs/drawer-navigator): Note `drawerLabel`, `drawerIcon`, `drawerActiveBackgroundColor`, and `drawerActiveTintColor`.
- [Native Stack Navigator](https://reactnavigation.org/docs/native-stack-navigator): Note `headerShown`, `headerStyle`, `headerTintColor`, and the `options` callback that receives `{ route }`.

**Key ideas to take away:**

- All three navigators share the same fundamental pattern: wrap everything in `NavigationContainer`, create a navigator object with `create*Navigator()`, then register screens with `Navigator` and `Screen` sub-components.
- Configuration follows the same shape across all three: `screenOptions` on the navigator applies to all screens; the `options` prop on an individual `Screen` overrides for that screen only.
- The `name` prop on each `Screen` is both the unique navigation identifier and the default display title. Choose it carefully, then use `headerTitle`, `tabBarLabel`, or `drawerLabel` to customise what users see.

---

## 3. Navigating Between Screens and Passing Data

**Read (20 min)**

- [React Navigation: Moving Between Screens](https://reactnavigation.org/docs/navigating): Read the full page. Focus on `navigation.navigate()` and `navigation.goBack()`.
- [React Navigation: Passing Parameters](https://reactnavigation.org/docs/params): Read the full page. Pay attention to how params are passed to `navigate()` and how they are read in the destination screen.

**Key ideas to take away:**

- `navigation.navigate('ScreenName')` moves to a named screen. If the screen is already in the stack, the Stack Navigator will navigate to the existing instance rather than creating a new one.
- Params are passed as a plain JavaScript object: `navigation.navigate('Detail', { id: 1 })`. They are accessed in the destination using `route.params`.
- The `navigation` prop is only injected into registered screens. Use the `useNavigation` hook to access it inside child components.

---

## 4. Screen Focus and State

**Read (15 min)**

- [React Navigation: Navigation Lifecycle](https://reactnavigation.org/docs/navigation-lifecycle): Read the full page. This explains when screens mount and unmount across different navigator types, which directly explains the state persistence behaviour covered in the lesson.
- [React Navigation: `useFocusEffect`](https://reactnavigation.org/docs/use-focus-effect): Read the usage example and the note about wrapping the callback in `useCallback`.

**Key ideas to take away:**

- In a Stack Navigator, navigating back from a screen pops it off the stack and unmounts it. Local state is lost.
- In Tab and Drawer Navigators, screens stay mounted when you switch away. Local state is preserved because the component is never unmounted.
- `useFocusEffect` runs a side effect when a screen gains focus. It is useful for refreshing data or resetting state each time a user arrives at a screen.

---

## 5. Watch: React Navigation Crash Course

**Watch (20 min)**

- [React Navigation v6 Crash Course](https://www.youtube.com/watch?v=OmQCU-3KPms): Watch the sections covering Stack, Tab, and Drawer navigators. You do not need to follow along with code; focus on seeing the navigators in action on a device so you recognise what each one looks and feels like before the lab.

---

## Reflection (5 min)

Before the lesson, write down answers to these three questions:

1. In your own words, what is the difference between the Stack Navigator and the Bottom Tab Navigator in terms of how they manage screen instances?
2. When would you choose a Drawer Navigator over a Bottom Tab Navigator for a mobile app?
3. What is one concept from the pre-reading that you would like the instructor to demonstrate more clearly?

Bring question 3 to class.
