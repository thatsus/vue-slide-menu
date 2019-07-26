<template>
  <div id="app">
    <button id="menu-button" type="button" @click="open = !open">
        &mdash;<br>&mdash;<br>&mdash;<br>&nbsp;
    </button>
    <img alt="Vue logo" src="./assets/logo.png" width="500">
    <VueSlideMenu :menu="menu" :open.sync="open" title="ThumbStopper" img="https://thumbstopper.com/wp-content/uploads/2019/06/ts-icon-mini-rev.svg"/>
  </div>
</template>

<script>
import VueSlideMenu from './components/VueSlideMenu.vue';

export default {
  name: 'app',
  components: {
    VueSlideMenu,
  },
  data() {
    return {
      open: false,
      menu: [
        {
          name: 'Admin',
          value: [
            {
              name: 'Resellers & Dealers',
              value: [
                { name: 'Resellers', value: '#' },
                { name: 'Dealerships', value: '#' },
                { name: 'Prospects', value: '#' },
                { name: 'Support Dashboard', value: '#' },
                { name: 'Data Providers', value: '#' },
                { name: 'Dealer Notifications', value: '#' },
                { name: 'Recent CL Activity', value: '#' },
              ],
            },
            {
              name: 'Social Admin',
              value: [
                { name: 'Unlinked Accounts', value: '#' },
                { name: 'Social Postmaster', value: '#' },
                { name: 'Social Advertising', value: '#' },
                { name: 'Brand Managers', value: '#' },
                { name: 'System Health', value: '#' },
              ],
            },
            {
              name: 'User Management',
              value: [
                { name: 'Users', value: '#' },
                { name: 'Groups', value: '#' },
                { name: 'Roles', value: '#' },
                { name: 'Rights', value: '#' },
              ],
            },
            {
              name: 'Admin Functions',
              value: [
                { name: 'Admin Search', value: '#' },
                { name: 'Invoices', value: '#' },
                { name: 'Advertisements', value: '#' },
                { name: 'Banner Templates', value: '#' },
                { name: 'Reseller Listings', value: '#' },
                { name: 'Services', value: '#' },
                { name: 'Sales Report', value: '#' },
                { name: 'Leads Report', value: '#' },
                { name: 'Other Settings', value: '#' },
              ],
            },
          ],
        },
        {
          name: 'Inventory',
          value: [
            {
              name: 'Inventory',
              value: [
                { name: 'Lot', value: '#' },
                { name: 'Add Vehicle', value: '#' },
                { name: 'Archived', value: '#' },
                { name: 'Window Stickers', value: '#' },
                { name: 'Download', value: '#' },
              ],
            },
          ],
        },
        {
          name: 'Marketplaces',
          value: [
            {
              name: 'Social Media',
              value: [
                { name: 'Dashboard', value: '#' },
                { name: 'Settings', value: '#' },
              ],
            },
            {
              name: 'eBay Classifieds',
              value: [
                { name: 'Dashboard', value: '#' },
                { name: 'Settings', value: '#' },
              ],
            },
          ],
        },
        {
          name: 'Leads',
          value: '#',
        },
        {
          name: 'Reports',
          value: [
            {
              name: 'Reports',
              value: [
                { name: 'All', value: '#' },
              ],
            },
          ],
        },
      ],
    };
  },
};
</script>

<style>
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
#menu-button {
    background-color: #eee;
    border: 3px solid blue;
    border-radius: 5px;
    color: blue;
    display: block;
    font-family: Arial, sans-serif;
    font-size: 32px;
    height: 46px;
    line-height: 10px;
    margin-left: 35px;
}
#menu-button:hover {
    background-color: #777;
}
#menu-button:active {
    background-color: #333;
}
</style>
