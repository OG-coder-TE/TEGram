<template>
  <div id="btns">
    <div v-if="isUsersPic">
      <a
        id="publicbtn"
        data-toggle="tooltip"
        title="Public view"
        v-if="!privated"
        @click="editPrivate(photo)"
        >&nbsp;<font-awesome-icon :icon="['fas', 'users']" />&nbsp;
      </a>
      <a
        id="privatebtn"
        data-toggle="tooltip"
        title="Private"
        v-else
        @click="editPrivate(photo)"
        >&nbsp;<font-awesome-icon :icon="['fas', 'user-shield']" />&nbsp;
      </a>
    </div>
  </div>
</template>

<script>
import photoService from "@/services/PhotoService";

export default {
  props: ["photo"],
  data() {
    return {
      privated: this.photo.private,
      isUsersPic: this.photo.userId == this.$store.state.user.id,
    };
  },

  methods: {
    editPrivate(photo) {
      photo.private = !photo.private;
      photoService
        .editPrivate(photo)
        .then((res) => {
          if (res.status === 201) {
            this.$store.commit("EDIT_PRIVATE", res.data);
            this.privated = this.photo.private;
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>

<style >
#btns {
  position: absolute;
  top: 5px;
  left: 5px;
  font-size: 13pt;
}

#btns:hover {
  -webkit-filter: drop-shadow(5px 5px 5px rgba(34, 34, 34, 0.904));
  filter: drop-shadow(5px 5px 5px rgba(34, 34, 34, 0.959));
  transform: scale(1.2, 1.2);
}
#publicbtn {
  color: dodgerblue;
}
#privatebtn {
  color: crimson;
}
</style>