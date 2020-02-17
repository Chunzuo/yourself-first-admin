<template>
  <div>
    <div class="vx-row">
      <vx-card title="Manage My Feedbacks">
        <vs-table :data="reviews" pagination max-items="10" search>
          <template slot="header">
            <!-- <vs-button class="mr-2">Add</vs-button> -->
            <vs-button
              class="mr-2"
              icon-pack="feather"
              icon="icon-plus"
              @click="showAddDialog"
              >Add New</vs-button
            >Total <strong>&nbsp;{{ reviews.length }}&nbsp;</strong> feedbacks
          </template>
          <template slot="thead">
            <vs-th sort-key="clientName">Client Name</vs-th>
            <vs-th sort-key="content">Content</vs-th>
            <vs-th></vs-th>
          </template>

          <template slot-scope="{ data }">
            <vs-tr :data="item" v-for="(item, index) in data" :key="index">
              <vs-td>{{ item.clientName }}</vs-td>
              <vs-td>{{ item.content }}</vs-td>
              <vs-td class="whitespace-no-wrap">
                <feather-icon
                  icon="EditIcon"
                  svgClasses="w-5 h-5 hover:text-primary stroke-current"
                  @click.stop="showEditDialog(item)"
                />
                <feather-icon
                  icon="TrashIcon"
                  svgClasses="w-5 h-5 hover:text-danger stroke-current"
                  class="ml-2"
                  @click.stop="showDeleteDialog(item)"
                />
              </vs-td>
            </vs-tr>
          </template>
        </vs-table>
      </vx-card>
    </div>

    <!-- Dialog -->
    <vs-prompt
      @cancel="closeDialog"
      @accept="acceptDialog"
      @close="closeDialog"
      :is-valid="validDialog"
      :active.sync="activeDialog"
      :title="dialogTitle"
    >
      <div class="con-exemple-prompt">
        Input client name and feedback to
        <b>continue</b>.
        <vs-input
          placeholder="Client Name"
          v-model="review.clientName"
          class="mt-4 mb-2 w-full"
        />

        <datepicker
          placeholder="Select Date"
          v-model="review.date"
          class="mb-2 w-full"
        ></datepicker>

        <vs-textarea
          label="Content"
          v-model="review.content"
          class="w-full"
          rows="6"
        />
      </div>
    </vs-prompt>
  </div>
</template>

<script>
import axios from "@/axios";
import Datepicker from "vuejs-datepicker";
export default {
  data() {
    return {
      reviews: [],
      currentPage: 1,
      activeDialog: false,
      review: {
        clientName: "",
        content: ""
      },
      dialogTitle: "Add New Feedback",
      mode: 1, // 1: Create, 2: Edit
      itemToDelete: null
    };
  },
  methods: {
    showAddDialog() {
      this.activeDialog = true;
      this.dialogTitle = "Add New Feedback";
      this.mode = 1;
    },
    acceptDialog() {
      this.$vs.loading();
      if (this.mode == 1) {
        axios
          .post("/review", this.review)
          .then(() => {
            this.review = {
              clientName: "",
              content: ""
            };
            this.$vs.loading.close();
          })
          .catch(error => {
            this.$vs.loading.close();
            alert(error.message);
          })
          .finally(() => {
            this.getFeedbackList();
          });
      } else {
        axios
          .put("/review", this.review)
          .then(() => {
            this.review = {
              clientName: "",
              content: ""
            };
            this.$vs.loading.close();
          })
          .catch(error => {
            this.$vs.loading.close();
            alert(error.message);
          })
          .finally(() => {});
      }
    },
    closeDialog() {
      this.review = {
        clientName: "",
        content: ""
      };
    },
    getFeedbackList() {
      this.$vs.loading();
      axios
        .get("/review")
        .then(response => {
          this.reviews = response.data;
        })
        .catch(error => {
          this.$vs.loading.close();
          alert(error.message);
        })
        .finally(() => {
          this.$vs.loading.close();
        });
    },
    showEditDialog(data) {
      this.review = data;
      this.activeDialog = true;
      this.mode = 2;
      this.dialogTitle = `Edit Feedback from ${data.clientName}`;
    },
    showDeleteDialog(data) {
      this.itemToDelete = data;
      this.$vs.dialog({
        type: "confirm",
        color: "danger",
        title: `Confirm`,
        text: `Are you sure delete feedback from ${data.clientName}?`,
        accept: this.deleteFeedback
      });
    },
    deleteFeedback() {
      this.$vs.loading();
      axios
        .delete("/review/" + this.itemToDelete._id)
        .then(() => {
          this.$vs.loading.close();
        })
        .catch(err => {
          this.$vs.loading.close();
          alert(err.message);
        })
        .finally(() => {
          this.getFeedbackList();
        });
    }
  },
  computed: {
    validDialog() {
      return (
        this.review.clientName.length > 0 && this.review.content.length > 0
      );
    }
  },
  components: {
    Datepicker
  },
  mounted() {
    this.getFeedbackList();
  }
};
</script>

<style lang="scss" scoped></style>
