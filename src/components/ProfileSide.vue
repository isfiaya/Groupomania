<template>
  <div class="wrap style">
    <div class="header">
      <img src="@/assets/icon-1.png" class="logo-side" alt="logo" />
    </div>
    <div class="main">
      <div class="items">
        <ul>
          <li>
            <router-link to="/home">
              <i class="fas fa-home"></i>Home
            </router-link>
          </li>
          <li>
            <i class="fas fa-image"></i>
            Photos
          </li>
          <li>
            <i class="fas fa-play"></i>
            Watch
          </li>
          <li>
            <i class="fas fa-user"></i>
            People
          </li>
          <li>
            <i class="fas fa-user-friends"></i>
            Groups
          </li>
          <li>
            <i class="fas fa-users"></i>
            Pages
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: "SideLeft",
};
</script>



<style lang="scss" scoped>
.wrap {
  overflow: auto;
  height: 100vh;
  background-color: #f8f9fb;
}
.style::-webkit-scrollbar-track {
  border-radius: 10px;
  background: transparent;
}
.style::-webkit-scrollbar {
  width: 12px;
  background: transparent;
}
.style::-webkit-scrollbar-thumb {
  border-radius: 10px;
  background-color: rgb(209, 203, 203);
}
.header {
  background-color: #383a45;
  height: 150px;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-image: linear-gradient(135deg, #4f5261 0%, #383a45 50%);
  img {
    width: 50px;
    height: auto;
    margin: auto;
  }
  p {
    color: #fff;
    margin-bottom: 0px !important;
  }
  p:nth-child(3) {
    font-size: small;
    opacity: 70%;
  }
}
.main {
  height: 100vh;
  text-align: center;
  padding-top: 20px;
  .items {
    height: 100%;
    margin-top: 10px;
    ul {
      display: flex;
      flex-direction: column;
      text-align: center;
      justify-content: center;
      align-items: center;
      flex-wrap: wrap;
      list-style: none;
      padding-left: 0;
      li {
        width: 100px;
        margin-bottom: 50px;
        border-radius: 7px;
        display: flex;
        flex-direction: column;
        color: hsl(0deg 0% 0% / 70%);
        cursor: pointer;
        padding-top: 3px;
        padding-bottom: 3px;

        a {
          color: none !important;
          display: flex;
          flex-direction: column;
        }
        a:hover {
          text-decoration: none;
        }
        a:visited {
          color: black;
        }
        i {
          margin-bottom: 10px;
        }
      }
      li:hover {
        background-color: #eee;
      }
    }
  }
}
</style>