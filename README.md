<a id="readme-top"></a>
<div align="center">

[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![License][license-shield]][license-url]

</div>

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <img src="logo.svg" alt="Logo" width="300"></p>
  <h3 align="center">Filtrs</h3>
  <p align="center">
    <br />
    A user-based <b> Collaborative Filtering </b> Implementation in <b> Rust </b>
  </p>
</div>

---

## Features

- **Generic Collaborative Filtering**: A user-based collaborative filtering algorithm.
- **Support for Multiple Interaction Types**: Our library is designed to handle various types of user interactions with items, even when they have different scales or metrics.

## How to use

Defining Core Components
```rs
/// Represents a user in the system
struct User {
    ...
}

/// Represents an item in the system
struct Item {
    ...
}

/// Represents an interaction between a user and an item
/// Must derive `Interact`
#[derive(Interact)]
struct Interaction {
    // For example 
    #[weight(80)]
    like: bool,
    #[weight(10)]
    score: u8,
    #[weight(120)]
    time: f32,
}
```

Managing Users and Items in the ``World``
```rs
// The World manages all users, items, and interactions
let mut world: World<(User, Item, Interaction)> = World::new();

// Add users and retrieve their unique identifiers (UUIDs)
let user1 = User;
let handle_user1: UserUuid = world.new_user(user1);
let user2 = User;
let handle_user2: UserUuid = world.new_user(user2);

// Remove a user from the world by their UUID
let user3 = User;
let handle_user3: UserUuid = world.new_user(user3)
world.remove_user(handle_user3);

// Retrieve users by their UUIDs. If the UUID does not exist, `None` is returned
assert_eq!(Some(user1), world.get_user(handle_user1));
assert_eq!(None, world.get_user(handle_user3);

// Add items and retrieve their unique identifiers (UUIDs)
let item1 = Item;
let handle_item1: ItemUuid = world.new_item(item1);
let item2 = Item;
let handle_item2: ItemUuid = world.new_item(item2);

// Remove an item from the world by its UUID
let item3 = Item;
let handle_item3: ItemUuid = world.new_item(item3);
world.remove_item(handle_item3);

// Retrieve items by their UUIDs. If the UUID does not exist, `None` is returned
assert_eq!(Some(item1), world.get_item(item1));
assert_eq!(None, world.get_item(handle_item3));

// Add or update an interaction between a user and an item.
// If an interaction already exists, it will be overwritten.
let interaction = Interaction;
let other_interaction = Interaction;
let last_interaction = Interaction;

world.iteract(handle_user1, handle_item1, interaction);
world.iteract(handle_user1, handle_item1, other_interaction);
world.iteract(handle_user1, handle_item2, other_interaction);
world.iteract(handle_user2, handle_item2, last_interaction);
assert!(world.has_interacted(handle_user2, handle_item2))

// Remove an interaction between a user and an item.
world.remove_interaction(handle_user1, handle_item2)

// Update global data structures in the world after changes:
world.update();
// Update preferences for a specific user without affecting the entire dataset.
world.update_user_preferences(handle_user1);
```

Analyzing and Querying the World
```rs
// Predict interactions between users and items:
// If an interaction already exists, `predict_interaction` returns the actual interaction.
let predict_interaction: Interaction = world.predict_interaction(UserUuid, ItemUuid);

// Get the top `n` recommended items for a user that they have not interacted with.
let top_items: Vec<ItemUuid> = world.get_top_items_for_user(UserUuid, n);

// Get the top `n` users most likely to interact with a given item.
let top_users: Vec<UserUuid> = world.get_top_users_for_item(ItemUuid, n);

// Query interactions:
let user_interactions: Vec<Interaction> = world.get_user_interactions(UserUuid);
let item_interactions: Vec<Interaction> = world.get_item_interactions(ItemUuid);

// Find `n` similar users or items based on interaction data:
let similar_users: Vec<UserUuid> = world.get_similar_users(UserUuid, n);
let similar_items: Vec<ItemUuid> = world.get_similar_items(ItemUuid, n);

// Retrieve the `n` most or least popular items:
let most_popular_items: Vec<ItemUuid> = world.get_most_popular_items(n);
let least_popular_items: ItemUuid = world.get_least_popular_items(n);

// Retrieve the `n` most or least satisfied user:
let most_satisfied_users: UserUuid = world.get_satisfiyest_users(n);
let least_satisfied_users: UserUuid = world.get_unsatisfiyest_users(n);

// Compare items for a user or users for an item:
let better_item: ItemUuid = world.compare_top_item_for_user(UserUuid, ItemUuid, ItemUuid);
let better_user: UserUuid = world.compare_top_item_for_user(ItemUuid, UserUuid, UserUuid);

// Count interactions:
let total_interactions: usize = world.get_interaction_count();
let user_interaction_count: usize = world.get_user_interaction_count(UserUuid);
let item_interaction_count: usize = world.get_item_interaction_count(ItemUuid);

// Compute average interactions per user:
let avg_interactions: f32 = world.get_average_interactions_per_user();

// Export interactions as a string for debugging or logging:
let interaction_dump: String = world.dump_interactions();
```

## License

This project is licensed under the GNU General Public License v3.0 (GPL-3.0).

![license](https://camo.githubusercontent.com/fdda5e4523c20de2843c4f9193639b82d93de44b0e87246865a71fdbaffe61d7/68747470733a2f2f75706c6f61642e77696b696d656469612e6f72672f77696b6970656469612f636f6d6d6f6e732f632f63622f47504c76335f4c6f676f5f66696c6c65642e706e67)

You are free to use, modify, and distribute this software under the terms of the GPL-3.0. A copy of the full license can be found [here](https://www.gnu.org/licenses/gpl-3.0.en.html).

<!-- PROJECT BADGES -->

[contributors-shield]: https://img.shields.io/github/contributors/cocosol007/filtrs.svg?style=for-the-badge
[contributors-url]: https://github.com/cocosol007/filtrs/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/cocosol007/filtrs.svg?style=for-the-badge
[forks-url]: https://github.com/cocosol007/filtrs/network/members
[stars-shield]: https://img.shields.io/github/stars/cocosol007/filtrs.svg?style=for-the-badge
[stars-url]: https://github.com/cocosol007/filtrs/stargazers
[issues-shield]: https://img.shields.io/github/issues/cocosol007/filtrs.svg?style=for-the-badge
[issues-url]: https://github.com/cocosol007/filtrs/issues
[license-shield]: https://img.shields.io/github/license/cocosol007/filtrs.svg?style=for-the-badge
[license-url]: https://github.com/cocosol007/filtrs/blob/main/LICENSE
