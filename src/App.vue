<template>
  <ul>
    <li>
      <router-link to="/">Halaman Auth</router-link>
    </li>
    <li>
      <router-link to="/todo">Halaman Todo</router-link>
    </li>
    <li>
      <router-link to="/user">Halaman User</router-link>
    </li>
  </ul>
  <div>
    <router-view></router-view>
  </div>
</template>