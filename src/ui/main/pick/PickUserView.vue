<template>
  <div class="pick-contact-container">
    <section class="contact-list-container">
      <div class="input-container">
        <input type="text" placeholder="搜索">
      </div>
      <div class="friend-list-container">
        <UserListVue :enable-pick="true" :users="users"/>
      </div>
    </section>
    <section class="checked-contact-list-container">
      <header>
        <h2>发起群聊</h2>
        <span v-if="sharedPickState.users.length === 0">已选择联系人</span>
        <span v-else>已选择联系人 {{ this.sharedPickState.users.length }}</span>
      </header>
      <div class="content">
        <div class="picked-user-container" v-for="(user, index) in sharedPickState.users" :key="index">
          <div class="picked-user">
            <img class="avatar" :src="user.portrait" :alt="user + index">
            <button @click="unpick(user)" class="unpick-button">x</button>
          </div>
          <span class="name single-line">{{ user.displayName }}</span>
        </div>
      </div>
      <footer>
        <button @click="cancel" class="cancel">取消</button>
        <button @click="confirm" class="confirm">创建</button>
      </footer>
    </section>
  </div>
</template>

<script>
import store from "@/store";
import UserListVue from "@/ui/main/user/UserListVue";

export default {
  name: "PickUserView",
  props: {
    users: {
      type: Array,
      required: true,
    },
    initialCheckedUsers: {
      type: Array,
      required: false,
      default: null,
    },
    title: {
      type: String,
      required: false,
      default: '',
    },
    confirmTitle: {
      type: String,
      required: false,
      default: '',
    }

  },
  data() {
    return {
      sharedPickState: store.state.pick,
    }
  },
  methods: {
    unpick(user) {
      store.pickUser(user, false);
    },

    cancel() {
      this.sharedPickState.users.length = 0
      this.$modal.hide('invite-modal', {confirm: false})
    },

    confirm() {
      let pickedUsers = [...this.sharedPickState.users];
      this.sharedPickState.users.length = 0
      this.$modal.hide('invite-modal', {confirm: true, users: pickedUsers})
    },
  },

  components: {UserListVue},
}
</script>

<style lang="css" scoped>
.pick-contact-container {
  display: flex;
  height: 100%;
  width: 100%;
}

.contact-list-container {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  background-color: #f7f7f7;
}

.contact-list-container .input-container {
  display: flex;
  width: 100%;
}

.contact-list-container input {
  height: 25px;
  margin: 15px 20px 15px 15px;
  flex: 1;
  border-radius: 3px;
  border: 1px solid #ededed;
  background-color: white;
  padding-left: 10px;
  text-align: left;
}

.contact-list-container .friend-list-container {
  height: 100%;
  overflow: auto;
  margin-left: -10px;
}

.checked-contact-list-container {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.checked-contact-list-container header {
  height: 55px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.checked-contact-list-container header h2 {
  font-size: 16px;
  font-weight: normal;
  margin-left: 30px;
}

.checked-contact-list-container header span {
  font-size: 12px;
  margin-right: 20px;
}


.checked-contact-list-container .content {
  height: 100%;
  flex: 1;
  display: flex;
  padding: 0 30px;
  flex-wrap: wrap;
  justify-content: flex-start;
  align-items: flex-start;
  align-content: flex-start;
  overflow: auto;
}

.checked-contact-list-container .content .picked-user-container {
  width: 33%;
  display: flex;
  flex-direction: column;
  column-count: 1;
  justify-content: center;
  align-content: center;
  padding: 5px 10px;
}

.checked-contact-list-container .content .picked-user-container .name {
  width: 100%;
  font-size: 12px;
}

.checked-contact-list-container .content .picked-user-container .picked-user {
  position: relative;
  height: 65px;
  width: 65px;
}

.checked-contact-list-container .content .avatar {
  width: 45px;
  height: 45px;
  margin: 10px 10px;
  border-radius: 3px;
  border: 1px solid red;
}

.checked-contact-list-container .content .unpick-button {
  position: absolute;
  width: 20px;
  height: 20px;
  border: 1px solid red;
  border-radius: 10px;
  top: 0;
  right: 0;
}

.checked-contact-list-container .content .unpick-button:active {
  background-color: #e5e5e5;
}

.checked-contact-list-container footer {
  height: 55px;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  margin-bottom: 10px;
}

.checked-contact-list-container footer button {
  padding: 5px 30px;
  border-radius: 4px;
  border: 1px solid #cccccc;
}

.checked-contact-list-container footer button.confirm {
  background-color: #20bf64;
  margin-left: 20px;
  margin-right: 20px;
}


</style>
