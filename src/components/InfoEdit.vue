<template>
  <div class="wrap">
    <nav>
      <ul>
        <li>
          <router-link to="../about">View</router-link>
        </li>
        <li>
          <router-link to="edit">Edit</router-link>
        </li>
      </ul>
    </nav>
    <div>
      <h2>View Profile</h2>
      <h3 class="sub-title">Base</h3>
    </div>
    <table class="table">
      <tbody>
        <!-- <tr>
          <td class="label">First Name</td>
          <td class="pl-4">
            <input type="text" v-model="firstName" class="field-1" />
          </td>
        </tr>-->
        <!-- <tr>
          <td class="label">Last Name</td>
          <td class="pl-4">
            <input type="text" v-model="lastName" class="field-1" />
          </td>
        </tr>-->
        <tr>
          <td class="label">Date of Birth</td>
          <td class="pl-4">
            <input type="date" ref="date" v-model="dateBrith" class="field-1" max="2018-12-31" />
          </td>
        </tr>
        <tr>
          <td class="label">Sex</td>
          <td class="pl-4 customColor d-flex">
            <div class="form-check mr-4">
              <input class="form-check-input" type="radio" value="Male" name="flexRadioDefault" id="flexRadioDefault1" v-model="gender" />
              <label class="form-check-label" for="flexRadioDefault1">Male</label>
            </div>
            <div class="form-check">
              <input class="form-check-input" type="radio" value="Female" name="flexRadioDefault" id="flexRadioDefault2" v-model="gender" />
              <label class="form-check-label" for="flexRadioDefault2">Female</label>
            </div>
          </td>
        </tr>
        <tr>
          <td class="label">City</td>
          <td class="pl-4">
            <input type="text" v-model="city" class="field-1" />
          </td>
        </tr>
        <tr>
          <td class="label">Country</td>
          <td class="pl-4 customColor">
            <input type="text" v-model="country" class="field-1" />
          </td>
        </tr>
      </tbody>
    </table>
    <div class="submit">
      <button class="btn-save-change" @click="submitInfoUser">Save Changes</button>
      <button class="btn-delete-account ml-4" @click="deleteAccount">Delete Account</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Swal from "sweetalert2";
export default {
  name: "InfoEdit",
  data() {
    return {
      // firstName: null,
      // lastName: null,
      city: null,
      country: null,
      dateBrith: null,
      gender: null,
    };
  },
  methods: {
    getUserData() {
      const id = this.$route.params.id;

      axios
        .post("https://social-network-groupmonia.herokuapp.com/home/users", {
          id: id,
        })
        .then((response) => {
          // this.firstName = response.data[0].first_name;
          // this.lastName = response.data[0].last_name;
          this.city = response.data[0].city;
          this.country = response.data[0].country;
          this.gender = response.data[0].gender;
          if (response.data[0].dateBrith) {
            this.dateBrith = response.data[0].dateBrith.split("T")[0];
          }
        });
    },
    submitInfoUser() {
      const id = localStorage.getItem("id");
      axios
        .post(
          "https://social-network-groupmonia.herokuapp.com/home/users/info",
          {
            id: id,
            // firstName: this.firstName,
            // lastName: this.lastName,
            dateBrith: this.dateBrith,
            gender: this.gender,
            city: this.city,
            country: this.country,
          }
        )
        .then((response) => {
          console.log(response);
          // localStorage.setItem("first_name", this.firstName);
          // localStorage.setItem("last_name", this.lastName);
          // this.$root.$refs.header.getOneUser();
          Swal.fire({
            icon: "success",
            title: "Your changes has been saved",
            showConfirmButton: false,
            timer: 1500,
          });
        });
    },
    deleteAccount() {
      Swal.fire({
        title: "Are you sure?",
        text: "You won't be able to revert this!",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "Yes, delete it!",
      }).then((result) => {
        if (result.isConfirmed) {
          const id = localStorage.getItem("id");
          axios
            .delete(
              "https://social-network-groupmonia.herokuapp.com/home/users/info",
              {
                data: {
                  id: id,
                },
              }
            )
            .then((response) => {
              console.log(response);
              localStorage.clear();
              this.$router.replace("/");
            });
          Swal.fire("Deleted!", "Your account has been deleted.", "success");
        }
      });
    },
  },
  created() {
    this.getUserData();
  },
};
</script>

<style lang="scss" scoped>
.wrap {
  margin-bottom: 5rem;
}
nav {
  margin-bottom: 1.5rem;
  ul {
    width: 100%;
    border-bottom: 1px solid #e7edf2;
    list-style: none;
    display: flex;
    padding-left: 0;
    li {
      margin-right: 20px;
      width: 40px;
      padding-bottom: 3px;
      text-align: center;
      a {
        color: #212529;
      }
      a:hover {
        text-decoration: none;
      }
    }
    li:nth-child(2) {
      border-bottom: 2px solid $Secondary-Color;
      a {
        color: $Primary-Color;
      }
      a:hover {
        text-decoration: none;
      }
    }
  }
}
.label {
  width: 35%;
  padding-top: 20px !important;
}
.customColor {
  color: $Secondary-Color;
}
h2 {
  font-size: 1.616em;
  color: #4f515b;
  margin-bottom: 1rem;
}
.sub-title {
  font-size: 1em !important;
  padding-bottom: 1rem;
  margin-bottom: 1rem;
  border-bottom: 1px solid #e7edf2;
}
.field-1 {
  background-color: #f7f7f7;
  display: block;
  height: 2.5rem;
  padding: 0.375rem 1rem;
  border: 1px solid transparent;
  border-radius: 1.25rem;
  outline: none;
}
.field-1:focus {
  border: 1px solid $Secondary-Color;
}
.submit {
  width: 100%;
  text-align: center;
  margin-top: 40px;
}
.btn-save-change {
  background-color: #051a38;
  // background-image: $background;
  box-shadow: 0 1px 2px 0 rgb(130 36 227 / 50%);
  color: #fff;
  border-radius: 30px;
  text-align: center;
  padding: 0.475rem 1rem;
}
.btn-save-change:hover {
  background-image: none;
}
.btn-save-change:active {
  transform: translateY(2px);
}
.btn-delete-account {
  background-color: #d63031;
  box-shadow: 0 1px 2px 0 rgb(130 36 227 / 50%);
  color: #fff;
  border-radius: 30px;
  text-align: center;
  padding: 0.475rem 1rem;
}
.btn-delete-account:active {
  transform: translateY(2px);
}
</style>