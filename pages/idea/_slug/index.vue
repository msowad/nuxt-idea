<template>
  <v-row>
    <v-col cols="12" md="12">
      <idea-card v-if="idea" :idea="idea" single />

      <v-card-actions v-if="idea.id" class="mt-5">
        <idea-reply :id="idea.id" />

        <idea-status
          v-if="$user.isAdmin()"
          :id="idea.id"
          :status="idea.status.name"
        />

        <v-spacer />
        <vote-button
          :user-id="idea.user_id"
          :votes="idea.votes_count"
          :idea-id="idea.id"
        />
      </v-card-actions>

      <idea-comments :idea-id="idea.id" />
    </v-col>
  </v-row>
</template>

<script>
import { mapActions, mapState } from 'vuex'

export default {
  async fetch() {
    await this.loadIdea(this.$route.params.slug)
  },

  computed: {
    ...mapState({
      idea: ({ idea }) => idea.idea,
    }),
  },

  methods: {
    created() {
      this.$router.push('/')
    },
    ...mapActions('idea', ['loadIdea']),
  },
}
</script>
