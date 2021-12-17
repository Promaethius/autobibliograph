<template>
    <div class="user-profile">
        <div class="user-profile__user-panel">
            <h1 class="user-profile__username">@{{ user.username }}</h1>
            <div class="user-profile__admin-badge" v-if="user.isAdmin">
                Admin
            </div>
            <div class="user-profile__folower-count">
                <strong>Followers: {{ followers }}</strong>
            </div>
			<form class="user-profile__create-twoot" @submit.prevent="createNewTwoot">
				<label for="newTwoot"><strong>New Twoot</strong></label>
				<textarea name="newTwoot" rows="4" v-model="newTwootContent"/>
				<div class="user-profile__create-twoot-type">
					<label for="newTwootType"><strong>Type:</strong></label>
					<select id="newTwootType" v-model="selectTwootType">
						<option :value="option.value" v-for="(option, index) in twootTypes" :key="index">
							{{ option.name }}
						</option>
					</select>
				</div>
				<button>
					Twoot!
				</button>
            </form>
        </div>
        <div class="user-profile__twoots-wrapper">
            <TwootItem v-for="twoot in user.twoots" :key="twoot.id" :username="user.username" :twoot="twoot" @favorite="toggleFavorite"/>
        </div>
    </div>
</template>

<script>
import TwootItem from "./TwootItem.vue"

export default {
  name: 'UserProfile',
  components: { TwootItem },
  data() {
    return {
		newTwootContent: '',
		selectTwootType: 'instant',
		twootTypes: [
			{value: 'draft', name: 'Draft'},
			{value: 'instant', name: 'Instant Twoot'}
		],
      followers: 0,
      user: {
        id: 1,
        username: 'promaethius',
        name: {
          first: 'Jonathan',
          last: 'Bryant'
        },
        email: 'bryant.jonathan.42@gmail.com',
        isAdmin: true,
        twoots:[
            {id: 1, content: 'Twotter is ok.'},
            {id: 2, content: 'Twotter is mediocre.'}
        ]
      }
    }
  },
  computed: {
    fullName() {
      return `${this.user.name.first} ${this.user.name.last}`;
    }
  },
  methods: {
    followUser() {
      this.followers++
    },
    toggleFavorite(id) {
      console.log(`Favorite twoot ${id}`)
    },
	createNewTwoot() {
		if (this.newTwootContent && this.selectTwootType !== 'draft') {
			this.user.twoots.unshift({
				id: this.user.twoots.length + 1,
				content: this.newTwootContent
			})
			this.newTwootContent = '';
		}
	}
  },
  mounted() {
    this.followUser();
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a follower.`)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.user-profile {
    display: grid;
    grid-template-columns: 1fr 3fr;
    width: 100%;
    padding: 50px 5%;
}

.user-profile__user-panel {
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #dfe3e8;
}

.user-profile__admin-badge {
    background: purple;
    color: white;
    border-radius: 5px;
    margin-right: auto;
    padding: 0 10px;
    font-weight: bold;
}

.user-profile__create-twoot {
	border-top: 1px solid #dfe3e8;
	display: flex;
	flex-direction: column;
	padding-top: 20px;
}

h1 {
    margin: 0;
}
</style>