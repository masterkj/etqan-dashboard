<template>
  <div>
    <div class="va-row">
      <div class="flex xs12 md12">
        <vuestic-widget headerText="all subjects">
          <div class="table-responsive">
            <table class="table table-striped first-td-padding">
              <thead>
                <tr>
                  <td>{{'cours-name' | translate}}</td>
                  <td>{{'start_date' | translate}}</td>
                  <td>{{'end_date' | translate}}</td>
                  <td>{{'actions' | translate}}</td>
                </tr>
              </thead>
              <tbody>
                <tr v-for="course in courses" :key="course.id">
                  <td>{{course.cours_name}}</td>
                  <td>{{course.start_date}}</td>
                  <td>{{course.end_date}}</td>
                  <td>
                    <button
                      type="button"
                      class="btn btn-info btn-sm"
                      @click="showCourseInfo(course)"
                    >
                      <i class="fas fa-info fa-sm"></i>
                    </button>
                    <button
                      type="button"
                      @click="addStudent(course.realcours_id, course.cours_id)"
                      class="btn btn-warning btn-sm ml-2"
                    >
                      <i class="fas fa-plus fa-xs"></i>
                    </button>

                    <button type="button" class="btn btn-success btn-sm ml-2">
                      <i class="far fa-money-bill-alt"></i>
                    </button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>

          <!-- <button class="btn btn-success" @click="showStaticModal()">{{'add' | translate }}</button> -->
        </vuestic-widget>
      </div>
    </div>

    <!---------------- course info -------------------------->

    <vuestic-modal
      :show.sync="show"
      v-bind:large="true"
      v-bind:force="true"
      ref="courseInfo"
      :cancelClass="'none'"
      :okText="'modal.close' | translate"
    >
      <div slot="title">{{'course info' | translate}}</div>
      <div>
        <div class="table-responsive">
          <table class="table table-striped first-td-padding">
            <thead>
              <tr>
                <td>{{'attribute' | translate}}</td>
                <td>{{'value' | translate}}</td>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>cours_name</td>
                <td>{{quiredCourse.cours_name}}</td>
              </tr>
              <tr>
                <td>start_date</td>
                <td>{{quiredCourse.start_date}}</td>
              </tr>
              <tr>
                <td>end_date</td>
                <td>{{quiredCourse.end_date}}</td>
              </tr>
              <tr>
                <td>price</td>
                <td>{{quiredCourse.price}}</td>
              </tr>
              <tr>
                <td>subject_id</td>
                <td>{{quiredCourse.cours_id}}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </vuestic-modal>

    <!------------- add student modal  --------------->

    <vuestic-modal
      :show.sync="show"
      v-bind:large="true"
      v-bind:force="true"
      ref="addStudentModal"
      :cancelClass="'none'"
      :okText="'modal.close' | translate"
    >
      <div slot="title">{{'add student' | translate}}</div>
      <div>
        <form>
          <div class="va-row">
            <div class="flex md4">
              <fieldset>
                <div class="form-group">
                  <div class="input-group">
                    <input id="simple-input" v-model="newStudent.user_id" required>
                    <label class="control-label" for="simple-input">
                      {{'user_id'
                      | translate}}
                    </label>
                    <i class="bar"></i>
                  </div>
                </div>
              </fieldset>
            </div>

            <div class="flex md4">
              <fieldset>
                <div class="form-group">
                  <div class="input-group">
                    <input id="simple-input" v-model="newStudent.payment" required>
                    <label class="control-label" for="simple-input">
                      {{'payment'
                      | translate}}
                    </label>
                    <i class="bar"></i>
                  </div>
                </div>
              </fieldset>
            </div>
            <div class="flex md4">
              <fieldset>
                <h5>
                  min pay: {{minPay}}
                  <br>
                  max pay: {{maxPay}}
                </h5>
              </fieldset>
            </div>
          </div>
        </form>
      </div>
    </vuestic-modal>
  </div>
</template>

<script>
export default {
  methods: {
    showStaticModal () {
      this.$refs.staticModal.open()
    },
    showCourseInfo (course) {
      this.quiredCourse = course
      this.$refs.courseInfo.open()
    },
    addStudent (groupId, subjectId) {
      this.minPay = 0
      this.maxPay = 0
      this.$http
        .get('/aletqan_project/api/min_max_payment.php')
        .then(response => {
          this.minPay = response.data.min_payment
          this.maxPay = response.data.max_payment
        })
      this.newStudent.group_id = groupId
      this.newStudent.course_id = subjectId
      this.$refs.addStudentModal.open()
    }
  },
  mounted () {
    this.$http
      .get('/aletqan_project/api/read_cours_Not_finished.php')
      .then(r => {
        this.courses = r.data
        console.log(r)
      })
  },
  name: 'Subjects',
  data () {
    return {
      show: true,
      quiredCourse: {},
      newStudent: {
        user_id: null,
        group_id: null,
        course_id: null,
        payment: null
      },
      minPay: 0,
      maxPay: 0,

      courses: [
        // {
        //   realcours_id: "10",
        //   cours_id: "23",
        //   Teacher_id: "2",
        //   start_date: "2019-05-16",
        //   end_date: "2019-07-16",
        //   price: "300",
        //   attendance_days: "dfdfdf",
        //   cours_name: "sss"
        // },
        // {
        //   realcours_id: "10",
        //   cours_id: "23",
        //   Teacher_id: "2",
        //   start_date: "2019-05-16",
        //   end_date: "2019-07-16",
        //   price: "300",
        //   attendance_days: "dfdfdf",
        //   cours_name: "sss"
        // }
      ]
    }
  },
  created () {
    this.$nextTick(() => {
      this.$validator.validateAll()
    })
  }
}
</script>

<style lang="scss" scoped>
.right {
  float: right;
}
.color-icon-label-table {
  td:first-child {
    width: 1rem;
  }
}

.modals-page {
  .modals-list {
    .btn {
      margin-right: 20px;
      margin-bottom: 25px;
    }
  }
}
</style>
