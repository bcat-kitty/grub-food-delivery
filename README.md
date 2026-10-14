# Grub — Cross-Platform Food Delivery App

Grub is a full-featured, UberEats-style food delivery app template that runs on **Android, iOS, desktop, and the web from a single Java/Kotlin codebase**. It's built on [Codename One](https://www.codenameone.com/) and the [CodeRAD](https://github.com/shannah/CodeRAD) library, and is designed as a clean, highly modular reference for building rich, reusable cross-platform UIs with a strict MVC separation.

## What it is

A production-shaped front-end for a food delivery experience: onboarding, authentication, restaurant discovery, menus, a cart with promo codes, saved addresses and payment cards, order placement and live order tracking, favorites, and a profile area — with light and dark themes throughout.

The app is intentionally **not** wired to a backend; all data is in-memory and exists for visual effect. Because the model layer is cleanly separated, swapping the in-memory models for real network/storage services is the main integration task.

## Features

- Light / dark mode
- Onboarding screens and first-time location setup
- Register & sign in
- Location selection and address management
- Search and multi-criteria filters
- Restaurant categories, restaurant pages, and dish detail with add-ons
- Cart, add-to-cart, and promo codes
- Payments: credit-card management
- Favorites
- Order placement and order tracking (processing / cancelled / completed)
- Profile editing
- Optimized for both iOS and Android, fully customizable, highly modularized

## Architecture

### Multi-module Maven build

The repo is a Codename One Maven project split into a shared module plus one module per build target:

```
.
├── common/      # ALL app code & resources — shared across every platform
├── android/     # Android build target
├── ios/         # iOS build target
├── javase/      # Desktop (JavaSE) + simulator target
├── javascript/  # Web build target
├── win/         # Windows UWP/device target
├── cn1libs/     # Bundled Codename One libraries (incl. CodeRAD)
└── pom.xml      # Parent POM
```

You write the app once in `common/`; the platform modules compile and package it natively for each OS.

### CodeRAD MVC layering

All application code lives under `common/src/main/java/com/codename1/demos/grub`, organized into four layers that keep UI, data, and navigation decoupled:

```
interfaces/   Entity contracts — Account, Restaurant, Dish, DishAddOn, DishOrder,
              CompletedOrder, CreditCard, PaymentMethod, Address, Filter,
              FoodCategory, MainWindow
      │
models/       Observable implementations of those interfaces (e.g. RestaurantModel,
              DishModel, AccountModel) that views bind to
      │
views/        Reusable UI components bound to models — RestaurantView, DishView,
              DishPreview, CompletedOrderView, AddressView, CreditCardView, …
      │
controllers/  One controller per screen/flow — SignIn/SignUp, Introduction,
              SetFirstLocation, MainWindow, Search, Filter, Restaurant, Dish,
              Order, Account, EditProfile, address & card editing
```

`Grub.java` is the entry point. Styling is defined with **Codename One CSS** under `common/src/main/css` (theme, colors, and category/window icon assets), so the look is tuned without touching Java. This is the Rapid Application Development pattern CodeRAD promotes: define an entity as an interface, back it with an observable model, bind a reusable view to it, and drive navigation from a controller.

## What stands out

- **One codebase, five targets** — native Android and iOS plus desktop and web, compiled from the same Java/Kotlin source via Codename One.
- **Textbook MVC via CodeRAD** — the interface→model→view→controller split makes screens reusable and the data layer swappable; wiring a real backend means replacing models, not rewriting UI.
- **CSS-driven theming** — light/dark and the entire visual language are expressed in Codename One CSS rather than hard-coded in components.
- **Breadth of a real app** — cart, promo codes, saved cards and addresses, order tracking, and favorites are all implemented, making it a realistic starting point rather than a toy demo.

## Tech stack

Java/Kotlin · Codename One (core, JavaSE) · CodeRAD · Maven (with `mvnw` wrapper) · Codename One CSS · Java 1.8 source level.

## How to run

### Prerequisites

- JDK 8 (the project targets Java 1.8)
- No separate Maven install needed — the bundled `mvnw` / `mvnw.cmd` wrapper is used by the scripts

### Build

```bash
mvn install        # or: ./mvnw install
```

### Run in the simulator or on desktop

Convenience scripts wrap the Codename One Maven goals:

```bash
./run.sh simulator   # launch the Codename One simulator (javase)
./run.sh desktop     # run as a desktop app
```

(On Windows use `run.bat`.) Under the hood these invoke, e.g., `./mvnw verify -Psimulator -Dcodename1.platform=javase`.

### Package native binaries

```bash
./build.sh windows_desktop   # Windows desktop build
./build.sh mac_desktop       # macOS desktop build
# (see build.sh / build.bat for ios, android, uwp and other targets)
```

### IDE

Open the project in any Maven-aware IDE (IntelliJ IDEA, VS Code, Eclipse, NetBeans) and run the `common` module.

A prebuilt Android demo APK is available from the repository's Releases.

> **Note:** The app is not connected to any backend; all logic is for visual effect. The MVC separation is designed to make wiring a server straightforward.

## License

Apache 2.0 — see [LICENSE](LICENSE).
