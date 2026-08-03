# 2.17 Application Flow Control with Navigation Frameworks

## Lesson Overview

This lesson teaches learners how to implement multi-screen navigation in React Native apps using React Navigation. Starting from the limitations of manual screen switching with `useState`, learners build the same set of screens three times using different navigator types: Bottom Tabs, Drawer, and Stack. By the end, they understand when to use each navigator, how to pass data between screens via route parameters, and how state persistence behaviour differs across navigator types.

## Dependencies

- [Self Studies](./studies.md)
- [Lesson](./lesson.md)
- [Assessment](./assessment.md)
- [Assignment](./assignment.md)

## Lesson Objectives

- Explain why dedicated navigation libraries are needed in React Native and how React Navigation compares to React Router in web apps
- Implement tab, drawer, and stack navigation using React Navigation, registering screens with `NavigationContainer` and the appropriate navigator
- Pass data between screens using route parameters and access the navigation API from any component using the `useNavigation` hook

## Lesson Plan

| Duration | What | How or Why |
|---|---|---|
| 10 min | Welcome and recap | Briefly revisit Lesson 2.16: React Native components, Expo setup; set context for today's focus on multi-screen apps |
| 40 min | Lecture: Navigation in React Native | Slides: why navigation libraries exist, React Navigation vs. React Router, navigator types overview, installing React Navigation |
| 5 min | Break | |
| 10 min | Setup and the naive approach | Code-along: scaffold `learn-navigation-app`, create shared screens and `Header` component, demonstrate the `useState` switcher and its limitations |
| 30 min | Lab Part 2: Tab Navigation | Code-along: install React Navigation, `createBottomTabNavigator`, register screens, Activity 1 (add Explore tab), create HotDeals screen, customise titles/labels/default screen/styling, Activity 2 (set icons for all tabs) |
| 20 min | Lab Part 3: Drawer Navigation | Code-along: install drawer + gesture/animation deps, build `DrawerNavigator`, customise titles, labels, styling, and icons |
| 5 min | Break | |
| 30 min | Lab Part 4: Stack Navigation | Code-along: install native stack, build `MenuScreen`, wire up navigation with the `navigation` prop, introduce `useNavigation`, pass route params, set dynamic header titles |
| 10 min | Lab Part 5: State Persistence | Demonstration: show state loss in Stack vs. state retention in Tabs/Drawer; introduce `useFocusEffect` to reset state on focus |
| 10 min | Lab Part 6: Nesting Navigators | Code-along: wrap `BottomTabsApp` inside a stack, `headerShown: false`, `ProductDetailScreen` accessible from any tab |
| 10 min | Wrap up and Q&A | Recap objectives, review when to use each navigator, preview Lesson 2.18 (native device capabilities) |
| **Total** | | **~180 min** |
| 30 min | Bonus (only if time permits): Part 7 Authenticated Navigation | Code-along: `AuthContext`, `LoginScreen`, `RegisterScreen`, split stacks, conditional navigator rendering, Logout button; not part of the core lesson plan, cover only if the class is ahead of schedule |
