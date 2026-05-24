# Optional Assignment: Recipe Browser App

## Overview

- **Lesson:** Application Flow Control with Navigation Frameworks / 2.17
- **Type:** Optional Take-Home Assignment
- **Estimated Time:** 2–3 hours
- **Due:** Before next lesson
- **Submission:** GitHub repository link or ZIP file

## Learning Objectives Covered

This assignment reinforces:

- Setting up and configuring React Navigation with a Bottom Tab Navigator and a Stack Navigator
- Passing data between screens using route parameters and reading them with `useRoute`
- Using `useNavigation` to access the navigation object from non-screen components
- Understanding how state persistence differs between navigator types

## Assignment Description

Build a **Recipe Browser** app with two tabs: a Browse tab that lists recipes by category, and a Favourites tab. Tapping a recipe navigates to a detail screen that shows the full recipe information. The focus is on composing a multi-screen app using the navigators and navigation patterns from the lesson.

### What You Will Build

- A **Browse tab** showing a list of recipe categories (for example, Breakfast, Lunch, Dinner, Snacks)
- A **Category screen** that shows recipe cards for the selected category
- A **Recipe Detail screen** that displays the recipe name, description, ingredients list, and estimated cook time
- A **Favourites tab** with a placeholder screen (implementing actual favourites storage is a bonus challenge)

## Requirements

### Core Requirements

#### 1. Project Setup

- [ ] Create a new Expo project: `npx create-expo-app --template blank RecipeBrowser`
- [ ] Install React Navigation and required packages:
  ```bash
  npm install @react-navigation/native @react-navigation/bottom-tabs @react-navigation/native-stack
  npx expo install react-native-screens react-native-safe-area-context
  ```
- [ ] Confirm the app runs on your device via Expo Go or the Android emulator

#### 2. Navigation Structure

Set up the following navigation hierarchy:

```
NavigationContainer
  Stack.Navigator (outer)
    Stack.Screen: "Tabs" → BottomTabsNavigator (headerShown: false)
    Stack.Screen: "RecipeDetail" → RecipeDetailScreen
  BottomTabsNavigator (inner)
    Tab.Screen: "Browse" → BrowseScreen
    Tab.Screen: "Favourites" → FavouritesScreen
```

- [ ] `RecipeDetailScreen` must be at the outer stack level so it can be reached from the Browse tab
- [ ] The tab navigator's header must be visible; the outer stack's header for the "Tabs" screen must be hidden with `headerShown: false`

#### 3. Browse Tab

- [ ] Display at least four recipe categories as tappable items (use `Button` or `Pressable`)
- [ ] Tapping a category navigates to `CategoryScreen`, passing the category name as a route param
- [ ] `CategoryScreen` displays the category name in the header and shows at least three hardcoded recipe items for that category
- [ ] Each recipe item is tappable and navigates to `RecipeDetailScreen`, passing at minimum: `name`, `description`, `cookTime`, and `fromCategory`

#### 4. Recipe Detail Screen

- [ ] Display the recipe name, description, cook time, and a list of at least three ingredients
- [ ] The header title must display the recipe name, set dynamically from `route.params`
- [ ] The iOS back button label must show the category name using `headerBackTitle`

#### 5. Favourites Tab

- [ ] Display a placeholder screen with a heading and a short message (for example, "No favourites yet")
- [ ] The tab must have a distinctive icon using `@expo/vector-icons`

#### 6. Styling

- [ ] Apply a consistent brand colour to the header and active tab using `screenOptions` on both navigators
- [ ] All screens must have a header with the brand colour background and white title text
- [ ] Use `StyleSheet.create()` for all styles

### Hardcoded Data

You may define your recipe data as a JavaScript object in a separate file, for example `data/recipes.js`:

```js
export const recipes = {
  Breakfast: [
    {
      id: 1,
      name: 'Avocado Toast',
      description: 'Creamy avocado on toasted sourdough.',
      cookTime: '10 min',
      ingredients: ['Sourdough bread', 'Avocado', 'Lemon juice', 'Salt', 'Chilli flakes'],
    },
    // add more recipes
  ],
  Lunch: [
    // ...
  ],
  // ...
};
```

Import this data in your screen components and filter by category name from `route.params`.

## Bonus Challenges

### Easy

- [ ] Add a tab icon for the Browse tab using `@expo/vector-icons`
- [ ] Add a `categoryScreen` screen-level header that shows the category name dynamically, set from `route.params` using the `Stack.Screen options` callback

### Medium

- [ ] Add a search input at the top of `BrowseScreen` that filters which categories are shown. Use `useFocusEffect` to clear the search query each time the user returns to the Browse tab.
- [ ] Add a "Back to Categories" button on `CategoryScreen` that calls `navigation.goBack()`. Style it as a text link rather than a default `Button`.

### Hard

- [ ] Implement a working Favourites feature. Add a "Add to Favourites" button on `RecipeDetailScreen`. Store favourites using `useState` in a React Context (review Lesson 2.6). Display the saved favourites on the Favourites tab. Removing a favourite should also be supported.
- [ ] Add a Drawer Navigator as an additional navigation layer. Register the tab navigator as one drawer item and add a separate "About" screen as another drawer item with a short description of the app.

## Submission

- [ ] Push your project to a GitHub repository
- [ ] Include a brief `README.md` in the repository with: how to install and run the app, a screenshot or screen recording of the navigation working, and any bonus challenges you completed

## Resources

- [React Navigation: Getting Started](https://reactnavigation.org/docs/hello-react-navigation)
- [React Navigation: Passing Parameters](https://reactnavigation.org/docs/params)
- [React Navigation: Native Stack Navigator](https://reactnavigation.org/docs/native-stack-navigator)
- [React Navigation: Bottom Tab Navigator](https://reactnavigation.org/docs/bottom-tab-navigator)
- [Expo Vector Icons](https://icons.expo.fyi)
