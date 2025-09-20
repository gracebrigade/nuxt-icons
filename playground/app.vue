<template>
  <div>
    <ul>
      <li>Pure icon: <nuxt-icon name="fire" /></li>
      <li class="colored">
        Icon with color from text: <nuxt-icon name="fire" />
      </li>
      <li>
        Icon with defs: <nuxt-icon name="css3" filled />
        <nuxt-icon name="javascript" filled />
      </li>
      <li>
        Subfolder icons: <nuxt-icon name="user/badge" /> User,
        <nuxt-icon name="admin/badge" /> Admin,
        <nuxt-icon name="admin/form/bug" />
      </li>
      <li>
        Reactive change:
        <nuxt-icon :name="icon ? 'user/badge' : 'admin/form/bug'" />
        <button @click="icon = !icon">
          Update
        </button>
      </li>
      <li class="colored">
        Inline icon (stroke) with color from text: <nuxt-icon name="foxcode-logo" />
      </li>
      <li class="colored">
        Icon with default fill: <nuxt-icon name="foxcode-logo" filled />
      </li>
      <li>
        <h3>icon-attr-aria-* Examples:</h3>
      </li>
      <li>
        Button with aria-label: 
        <button>
          <nuxt-icon name="fire" icon-attr-aria-label="Delete item" icon-attr-role="img" />
          Delete
        </button>
      </li>
      <li>
        Navigation icon with aria-hidden: 
        <nuxt-icon name="airplane" icon-attr-aria-hidden="true" />
        <span>Travel</span>
      </li>
      <li>
        Icon with aria-describedby: 
        <nuxt-icon name="user/badge" icon-attr-aria-describedby="user-description" icon-attr-role="img" />
        <span id="user-description">User profile badge</span>
      </li>
      <li>
        Interactive icon with multiple aria attributes:
        <button @click="toggleFavorite" class="favorite-btn">
          <nuxt-icon 
            name="fire" 
            icon-attr-aria-label="Add to favorites"
            icon-attr-aria-pressed="false"
            icon-attr-role="button"
            icon-attr-tabindex="0"
          />
          {{ isFavorite ? "Remove from" : "Add to" }} favorites
        </button>
      </li>
      <li>
        Status icon with aria-live region:
        <nuxt-icon 
          name="css3" 
          icon-attr-aria-label="Processing status"
          icon-attr-aria-live="polite"
          icon-attr-role="status"
        />
        <span>Processing...</span>
      </li>
      <li>
        Icon with custom aria-expanded (for dropdown):
        <button @click="toggleDropdown" class="dropdown-btn">
          <nuxt-icon 
            name="admin/badge" 
            :icon-attr-aria-expanded="isDropdownOpen ? 'true' : 'false'"
            icon-attr-aria-haspopup="true"
            icon-attr-role="button"
          />
          Menu {{ isDropdownOpen ? "(open)" : "(closed)" }}
        </button>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref } from '#imports'

const icon = ref(false)
const isFavorite = ref(false)
const isDropdownOpen = ref(false)

function toggleFavorite() {
  isFavorite.value = !isFavorite.value
}

function toggleDropdown() {
  isDropdownOpen.value = !isDropdownOpen.value
}
</script>

<style>
.colored {
  color: orange;
}

h3 {
  margin: 20px 0 10px 0;
  color: #2c3e50;
  font-size: 1.2em;
}

.favorite-btn, .dropdown-btn {
  padding: 8px 12px;
  margin: 5px;
  border: 2px solid #3498db;
  background: #ecf0f1;
  border-radius: 4px;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  transition: all 0.2s;
}

.favorite-btn:hover, .dropdown-btn:hover {
  background: #3498db;
  color: white;
}

.favorite-btn:active, .dropdown-btn:active {
  transform: scale(0.98);
}

li {
  margin: 8px 0;
  display: flex;
  align-items: center;
  gap: 8px;
}

button {
  display: inline-flex;
  align-items: center;
  gap: 4px;
}
</style>
