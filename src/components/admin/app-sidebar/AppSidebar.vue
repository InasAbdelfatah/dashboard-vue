<template>
  <vuestic-sidebar :hidden="isOpen">
    <template slot="menu">
      <sidebar-link
        :to="{ name: 'dashboard' }">
        <span slot="title">
          <span
            class="sidebar-menu-item-icon vuestic-icon vuestic-icon-dashboard"></span>
          <span>{{ $t('menu.dashboard') }}</span>
        </span>
      </sidebar-link>

      <sidebar-link
          :to="{ name: 'countries' }">
          <span slot="title">
            <span
            class="sidebar-menu-item-icon vuestic-icon vuestic-icon-tables"></span>
            <span>{{ $t('menu.countries') }}</span>
          </span>
        </sidebar-link>
 
      <sidebar-link-group>
        <span slot="title">
          <span
            class="sidebar-menu-item-icon vuestic-icon vuestic-icon-ui-elements"></span>
          <span>{{ $t('menu.countries') }}</span>
        </span>
        <sidebar-link
          :to="{ name: 'countries' }">
          <span slot="title">
            <span>{{ $t('menu.countries') }}</span>
          </span>
        </sidebar-link>
        <sidebar-link
          :to="{ name: 'countries' }">
          <span slot="title">
            <span>{{ $t('menu.countries') }}</span>
          </span>
        </sidebar-link>
      </sidebar-link-group>
 
    </template>
  </vuestic-sidebar>
</template>

<script>

import VuesticSidebar
  from '../../../vuestic-theme/vuestic-components/vuestic-sidebar/VuesticSidebar'
import SidebarLink from './components/SidebarLink'
import SidebarLinkGroup from './components/SidebarLinkGroup'

export default {
  name: 'app-sidebar',
  components: {
    VuesticSidebar,
    SidebarLink,
    SidebarLinkGroup,
  },
  props: {
    isOpen: {
      type: Boolean,
      required: true,
    },
  },
}

</script>
