<template>
  <div class="steam-friend" :key="friend.SteamID">
    <img
      :src="friend.avatarUrl"
      @error="imgErr"
      class="steam-friend-avatar"
      draggable="false"
    />
    <div class="steam-friend-info">
      <div class="steam-friend-name">{{ friend.personaName }}</div>
      <div :class="['steam-friend-status', statusColor]">{{ statusText }}</div>
    </div>
    <div class="steam-friend-right" @click="() => openChat()">
      <i class="fas fa-envelope"></i>
      <md-tooltip>{{ $t("#UI_SEND_MESSAGE") }}</md-tooltip>
    </div>
  </div>
</template>

<script type="text/javascript">
import { shell } from "electron";

export default {
  name: "steam-friend",
  props: {
    friend: {
      type: Object,
      default: () => {},
    },
  },
  methods: {
    openChat() {
      shell.openExternal(`steam://friends/message/${this.friend.friendID.id}`);
    },
    imgErr(e) {
      e.target.src = require("@/assets/noavatar.png");
    },
  },
  computed: {
    statusText() {
      switch (this.friend.priority) {
        case -1:
          return this.$t("#UI_IN_LAUNCHER", this.friend.friendRPC);
        case 0:
          return this.$t("#UI_IN_HL");
        case 1:
          return this.$t("#UI_IN_OTHER_GAME");
        case 2:
          return this.$t("#UI_ONLINE");
        case 3:
          return this.$t("#UI_OFFLINE");
      }
    },
    statusColor() {
      switch (this.friend.priority) {
        case -1:
          return "steam-yellow";
        case 0:
          return "steam-orange";
        case 1:
          return "steam-blue";
        case 2:
          return "steam-green";
        case 3:
          return "steam-white";
      }
    },
  },
};
</script>

<style scoped>
.steam-friend {
  display: flex;
  align-items: center;
  height: 57px;
  padding: 0 16px;
  overflow: hidden;
}

.steam-friend:hover {
  background: rgba(255, 255, 255, 0.05);
  transition: 100ms;
}

.steam-friend-avatar {
  width: 44px;
  height: 44px;
  background: rgba(255, 255, 255, 0.2);
  background-position: center !important;
  background-size: cover !important;
  border-radius: 50%;
  box-shadow: 0 0 4px rgba(0, 0, 0, 0.3);
}

.steam-friend-info {
  color: rgb(240, 240, 240) !important;
  margin-left: 8px;
}

.steam-orange {
  color: #ff880d;
}

.steam-green {
  color: #28d04c;
}

.steam-yellow {
  color: #d0cb28;
}

.steam-blue {
  color: #2490dc;
}

.steam-white {
  color: rgb(150, 150, 150);
}

.steam-friend-right {
  margin-left: auto;
  font-size: 20px;
  color: white;
  opacity: 0.2;
  cursor: pointer;
  transition: all 50ms ease-in-out;
}

.steam-friend-right:hover {
  opacity: 0.3;
}
</style>
