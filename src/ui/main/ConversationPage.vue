<template>
  <section class="conversation-page">
    <ConversationListPanel class="conversation-list-panel"/>
    <ConversationView class="conversation-view"/>
  </section>
</template>

<script>
import ConversationView from "./conversation/ConversationView";
import ConversationListPanel from "@/ui/main/ConversationListPanel";

export default {
  name: "ConversationPage",
  data() {
    return {
      currentConversation: null,
      conversationList: [1, 2, 3, 4, 5, 6, 7]
    };
  },
  destroyed() {
    console.log('conversation page destroyed')
  },

  methods: {
    showConversation(conversation) {
      this.currentConversation = conversation;
      console.log("show conversation", conversation);
    },
  },
  components: {
    ConversationListPanel,
    ConversationView,
  },
};
</script>

<style lang="css" scoped>
.conversation-page {
  flex: 1;
  display: flex;
  height: 100%;
}

.conversation-list-panel {
  width: 250px;
  height: 100%;
}

.conversation-view {
  flex: 1;
}

</style>
