<template>
  <li class="comment">
    <div class="info">
      <router-link :to="{
        name:'profile',
        params:{id:id}
      }">
        <img src="@/assets/user.png" alt="userImage" v-if="!img" />
        <img :src="img" alt="userImage" v-if="img" />
      </router-link>

      <router-link :to="{
        name:'profile',
        params:{id:id}
      }">{{firstName + " " + lastName}}</router-link>
    </div>
    <div class="inputComment" :class="{boxShadow:edit}">
      <span class="inputComment-Comment" v-if="!edit">{{message}}</span>
      <input type="text" class="inputComment-Comment" v-model="message" v-if="edit" ref="comment" />
      <div class="iconSend">
        <i class="fas fa-paper-plane icon-send-edit" v-if="edit" @click="sendUpdatedComment"></i>
        <span class="tooltiptext">Update Comment</span>
      </div>
    </div>
    <div class="tools" v-if="userCommentId">
      <button class="mr-3" @click="showEditComment">Edit</button>
      <button class="mr-3" @click="cancelEdit" v-if="edit">Cancel</button>
      <button @click="deleteComment">Delete</button>
    </div>
  </li>
</template>

<script>
import axios from "axios";
export default {
  name: "Comment",
  data() {
    return {
      firstName: null,
      lastName: null,
      userCommentId: false,
      img: null,
      edit: false,
      message: this.msg,
      CommentID: this.idComment,
    };
  },
  props: ["msg", "id", "postId", "idComment"],
  methods: {
    deleteComment() {
      axios
        .delete(
          "https://social-network-groupmonia.herokuapp.com/home/comment",
          {
            data: {
              idComment: this.idComment,
            },
          }
        )
        .then((response) => {
          console.log(response);
          this.getComment();
          this.$store.commit("showToastDeleteComment");
        });
    },
    getComment() {
      this.$parent.getComment();
    },
    showEditComment() {
      this.edit = true;
      setTimeout(() => {
        this.$refs.comment.focus();
      }, 100);
    },
    sendUpdatedComment() {
      const newComment = this.message;
      if (!newComment) {
        return this.deleteComment();
      }
      axios
        .post(
          "https://social-network-groupmonia.herokuapp.com/home/comment/edit",
          {
            id: this.CommentID,
            comment: newComment,
          }
        )
        .then((response) => {
          console.log(response);
          this.getComment();
          this.edit = false;
        });
    },
    cancelEdit() {
      const previousMessage = this.msg;
      this.edit = false;
      this.message = previousMessage;
    },
  },
  // mounted() {
  //   if (this.edit) {
  //     this.$refs.comment.focus();
  //   }
  // },
  created() {
    const userId = this.id;
    const userConnect = localStorage.getItem("id");
    axios
      .get("https://social-network-groupmonia.herokuapp.com/home/users")
      .then((response) => {
        const data = response.data;
        const dataFilter = data.filter((user) => user.id == parseInt(userId));
        this.firstName = dataFilter[0].first_name;
        this.lastName = dataFilter[0].last_name;
        this.img = dataFilter[0].imageUser;
        if (dataFilter[0].id == userConnect) {
          this.userCommentId = true;
        }
      });
  },
};
</script>

<style lang="scss" scoped>
.comment {
  display: flex;
  flex-direction: column;
  .info {
    display: flex;
    align-items: center;
    a {
      color: black;
      text-decoration: none;
    }
    img {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      border: 1px solid #eeeeee;
      margin-right: 15px;
    }
  }
  .inputComment {
    background-color: #eeeeee;
    border-radius: 20px;
    padding: 10px;
    margin-left: 40px;

    input {
      outline: none;
      width: 90%;
    }
    .icon-send-edit {
      float: right;
      padding: 4px;
    }
  }

  .tools {
    display: flex;
    margin-top: 10px;
    margin-left: 40px;
    p {
      margin-right: 15px;
    }
  }
}
.wrapComment {
  display: flex;
  align-items: center;
  .imgComment {
    width: 10%;
    img {
      max-width: 40px;
      border-radius: 50%;
      height: auto;
    }
  }
  .inputComment {
    width: 70%;
    input {
      padding: 7px;
      width: 100%;
      border-radius: 30px;
      outline: none;
      border: 2px solid #e7edf2;
      background-color: #eee;
    }
  }
  .btnComment {
    margin-left: 20px;

    button {
      padding: 7px 20px 7px 20px;
      border-radius: 20px;
      outline: none;
      background-color: #8224e3;
      background-image: linear-gradient(
        90deg,
        #8224e3 0,
        #a968ec 50%,
        #8224e3 100%
      );
      box-shadow: 0 1px 2px 0 rgb(130 36 227 / 50%);
      color: #fff;
    }
  }
}
.boxShadow {
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
}
.iconSend {
  position: relative;
  float: right;
}
.tooltiptext {
  visibility: hidden;
  width: 120px;
  background-color: black;
  color: #fff;
  font-size: 12px !important;
  text-align: center;
  padding: 5px 0;
  border-radius: 6px;
  position: absolute;
  z-index: 1;
  top: 100%;
  left: 50%;
  margin-left: -60px;
  margin-top: 15px;
}
.iconSend:hover {
  .tooltiptext {
    visibility: visible;
  }
}
</style>