/* eslint-disable space-before-function-paren */
<template>
  <div>
    <div class="va-row">
      <div class="flex xs12 md12">
        <vuestic-widget headerText="all subjects">
          <div class="table-responsive">
            <table class="table table-striped first-td-padding">
              <thead>
                <tr>
                  <td>{{'subject-name' | translate}}</td>
                  <td>{{'have-exam' | translate}}</td>
                  <td>{{'practical-mark' | translate}}</td>
                  <td>{{'Actions' | translate}}</td>
                </tr>
              </thead>
              <tbody>
                <tr v-for="subject in subjects" :key="subject.id">
                  <td>{{subject.cours_name}}</td>
                  <td>{{have_exam(subject.have_exam)}}</td>
                  <td>{{subject.Practical_mark}}</td>
                  <td>
                    <button type="button" class="btn btn-info btn-sm" @click="showInfo(subject)">
                      <i class="fas fa-info fa-sm"></i>
                    </button>
                    <button
                      type="button"
                      class="btn btn-warning btn-sm ml-2"
                      @click="addCourse(subject.id)"
                    >
                      <i class="fas fa-plus fa-xs"></i>
                    </button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>

          <button class="btn btn-success" @click="showStaticModal()">{{'add' | translate }}</button>
        </vuestic-widget>
      </div>
    </div>
    <!-- add subject modal -->

    <vuestic-modal
      :show.sync="showAddSubject"
      v-bind:large="true"
      v-bind:force="true"
      ref="staticModal"
      :cancelClass="'none'"
      :okText="'modal.close' | translate"
    >
      <div slot="title">{{'add a sbject' | translate}}</div>
      <div>
        <form>
          <div class="va-row">
            <div class="flex md4">
              <fieldset>
                <div class="form-group">
                  <div class="input-group">
                    <input id="simple-input" v-model="newSubject.fullname" req
                    shortname: this.fullnameuired>
                    <label
                      class="control-label"
                      for="simple-input"
                    >
                      {{'subject-name'
                      | translate}}
                    </label>
                    <i class="bar"></i>
                  </div>
                </div>

                <div class="form-group">
                  <div class="input-group">
                    <textarea
                      type="text"
                      v-model="newSubject.summary"
                      id="simple-textarea"
                      required
                    ></textarea>
                    <label class="control-label" for="simple-textarea">
                      {{'subject-description'
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
                    <input
                      id="simple-input"
                      :disabled="!newSubject.have_exam"
                      v-model="newSubject.marker"
                      required
                    >
                    <label class="control-label" for="simple-input">
                      {{'course-mark'
                      | translate}}
                    </label>
                    <i class="bar"></i>
                  </div>
                </div>

                <div class="form-group">
                  <div class="input-group">
                    <input v-model="newSubject.Practical_mark" id="simple-input" required>
                    <label class="control-label" for="simple-input">
                      {{'practical-mark'
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
                  <vuestic-switch v-model="newSubject.have_exam">
                    <span slot="trueTitle">{{'exam' | translate}}</span>
                    <span slot="falseTitle">{{"not" | translate}}</span>
                  </vuestic-switch>
                </div>
                <div class="form-group">
                  <div class="input-group">
                    <input id="simple-input" v-model="newSubject.categoryId" required>
                    <label class="control-label" for="simple-input">
                      {{'category-id'
                      | translate}}
                    </label>
                    <i class="bar"></i>
                  </div>
                </div>
                <div class="form-group">
                  <button
                    class="btn btn-success"
                    @click="submitNewSubject()"
                  >{{'submit' | translate }}</button>
                </div>
              </fieldset>
            </div>
          </div>
        </form>
      </div>
    </vuestic-modal>

    <!---------        subject info modal       ----------->
    <vuestic-modal
      :show.sync="showSubjectInfo"
      v-bind:large="true"
      v-bind:force="true"
      ref="subjectInfoModal"
      :cancelClass="'none'"
      :okText="'modal.close' | translate"
    >
      <div slot="title">{{'subject info' | translate}}</div>
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
                <td>course-name</td>
                <td>{{quiredSubject.fullname}}</td>
              </tr>
              <tr>
                <td>category-id</td>
                <td>{{quiredSubject.category}}</td>
              </tr>
              <tr>
                <td>summary</td>
                <td>{{quiredSubject.summary}}</td>
              </tr>
              <tr>
                <td>numsections</td>
                <td>{{quiredSubject.numsections}}</td>
              </tr>
              <tr>
                <td>have_exam</td>
                <td>{{have_exam(quiredSubject.have_exam)}}</td>
              </tr>
              <tr>
                <td>Practical_mark</td>
                <td>{{quiredSubject.Practical_mark}}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </vuestic-modal>

    <!---------        add course modal       ----------->
    <vuestic-modal
      :show.sync="showAddCourse"
      v-bind:large="true"
      v-bind:force="true"
      ref="addCourseModal"
      :cancelClass="'none'"
      :okText="'modal.close' | translate"
    >
      <div slot="title">{{'add course' | translate}}</div>
      <div>
        <form>
          <div class="va-row">
            <div class="flex md4">
              <fieldset>
                <div class="form-group">
                  <div class="input-group">
                    <input id="simple-input" v-model="newCourse.name" required>
                    <label class="control-label" for="simple-input">
                      {{'course-name'
                      | translate}}
                    </label>
                    <i class="bar"></i>
                  </div>
                </div>

                <div class="form-group">
                  <div class="input-group">
                    <input id="simple-input" v-model="newCourse.course_id" required>
                    <label class="control-label" for="simple-input">
                      {{'course_id'
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
                    <input id="simple-input" v-model="newCourse.Teacher_id" required>
                    <label class="control-label" for="simple-input">
                      {{'Teacher-id'
                      | translate}}
                    </label>
                    <i class="bar"></i>
                  </div>
                </div>

                <div class="form-group">
                  <div class="input-group">
                    <textarea
                      type="text"
                      v-model="newCourse.description"
                      id="simple-textarea"
                      required
                    ></textarea>
                    <label class="control-label" for="simple-textarea">
                      {{'course-description'
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
                    <vuestic-date-picker
                      id="date-picker-custom-first-day"
                      :config="{locale: {firstDayOfWeek: 1}}"
                      v-model="newCourse.start_date"
                    />
                    <label class="control-label" for="date-picker-custom-first-day">start-date</label>
                    <i class="bar"></i>
                  </div>
                </div>

                <div class="form-group">
                  <div class="input-group">
                    <vuestic-date-picker
                      id="date-picker-custom-first-day"
                      :config="{locale: {firstDayOfWeek: 1}}"
                      v-model="newCourse.end_date"
                    />
                    <label class="control-label" for="date-picker-custom-first-day">end-date</label>
                    <i class="bar"></i>
                  </div>
                </div>

                <div class="form-group">
                  <button
                    class="btn btn-success"
                    @click="submitNewCourse()"
                  >{{'submit' | translate }}</button>
                </div>
              </fieldset>
            </div>
          </div>
        </form>
      </div>
    </vuestic-modal>

    <!------- success modal ------------>
    <vuestic-modal
      :show.sync="showSuccessModal"
      v-bind:large="true"
      v-bind:force="true"
      ref="successModal"
      :cancelClass="'none'"
      :okText="'modal.close' | translate"
    >
      <div slot="title">{{'success request' | translate}}</div>
      <div>
        <p>your request go success</p>
      </div>
    </vuestic-modal>

    <vuestic-modal
      :show.sync="showErrorModal"
      v-bind:large="true"
      v-bind:force="true"
      ref="errorModal"
      :cancelClass="'none'"
      :okText="'modal.close' | translate"
    >
      <div slot="title">{{'error ' | translate}}</div>
      <div>
        <p>{{error}}</p>
      </div>
    </vuestic-modal>
  </div>
</template>

<script>
export default {
  computed: {
    isSuccessfulEmailValid () {
      let isValid = false
      if (this.formFields.successfulEmail) {
        isValid =
          this.formFields.successfulEmail.validated &&
          this.formFields.successfulEmail.valid
      }
      return isValid
    }
  },

  methods: {
    showStaticModal () {
      this.$refs.staticModal.open()
    },
    clear (field) {
      this[field] = ''
    },
    submitNewSubject () {
      this.$http
        .post('/aletqan_project/api/create_course.php', this.newSubject)
        .then(r => {
          if (
            r.data.message === 'Unable to create product. Data is incomplete.'
          ) {
            this.error = 'there is wrong in your inputs'
            this.$refs.errorModal.open()
          } else this.$refs.successModal.open()
        })
        .catch(() => {
          this.error = 'connection error'
          this.$refs.errorModal.open()
        })
    },
    submitNewCourse () {
      this.$http
        .post('/aletqan_project/api/create_realcourse.php', this.newCourse)
        .then(r => {
          if (r.data.message === 'group was created.') { this.$refs.successModal.open() } else {
            this.error = 'there is wrong in your inputs'
            this.$refs.errorModal.open()
          }
        })
        .catch(() => {
          this.error = 'connection error'
          this.$refs.errorModal.open()
        })
    },

    have_exam (num) {
      if (num === 0) return false
      else return true
    },
    showInfo (subject) {
      this.$refs.subjectInfoModal.open()
      this.quiredSubject = subject
    },
    addCourse (id) {
      this.$refs.addCourseModal.open()
      this.addedCourseSubjectId = id
    }
  },

  mounted () {
    this.$http.get('/aletqan_project/api/read_all_courses.php').then(r => {
      this.subjects = r.data
    })
  },
  name: 'Subjects',
  data () {
    return {
      showAddSubject: true,
      showSubjectInfo: true,
      showAddCourse: true,
      showSuccessModal: true,
      showErrorModal: true,

      error: '',
      newSubject: {
        have_exam: true,
        fullname: '',
        shortname: this.fullname,
        summary: '',
        Practical_mark: null,
        marker: null,
        category: 0
      },

      quiredSubject: {},
      newCourse: {
        course_id: null,
        Teacher_id: 0,
        start_date: 0,
        end_date: 0,
        attendance_days: 0,
        name: '',
        price: '',
        description: ''
      },

      subjects: [
        // {
        //   id: '1',
        //   fullname: 'ALETQAN',
        //   shortname: 'etqan',
        //   displayname: 'ALETQAN',
        //   categoryid: '0',
        //   summary: 'welcome!!</p>',
        //   summaryformat: '1',
        //   format: 'site',
        //   startdate: '0',
        //   enddate: '0',
        //   numsections: 1,
        //   categorysortorder: '1',
        //   idnumber: '',
        //   showgrades: '1',
        //   showreports: '0',
        //   newsitems: '3',
        //   visible: '1',
        //   maxbytes: '0',
        //   groupmode: '0',
        //   groupmodeforce: '0',
        //   defaultgroupingid: '0',
        //   lang: '',
        //   timecreated: '1557059677',
        //   timemodified: '1557062764',
        //   forcetheme: '',
        //   enablecompletion: '0',
        //   completionnotify: '0',
        //   courseformatoptions: [
        //     {
        //       name: 'numsections',
        //       value: 1
        //     }
        //   ],
        //   cours_name: 'compilar17',
        //   have_exam: '0',
        //   Practical_mark: '130'
        // },
        // {
        //   id: '2',
        //   fullname: 'ALETQAN',
        //   shortname: 'etqan',
        //   displayname: 'ALETQAN',
        //   categoryid: '0',
        //   summary: 'welcome!!</p>',
        //   summaryformat: '1',
        //   format: 'site',
        //   startdate: '0',
        //   enddate: '0',
        //   numsections: 1,
        //   categorysortorder: '1',
        //   idnumber: '',
        //   showgrades: '1',
        //   showreports: '0',
        //   newsitems: '3',
        //   visible: '1',
        //   maxbytes: '0',
        //   groupmode: '0',
        //   groupmodeforce: '0',
        //   defaultgroupingid: '0',
        //   lang: '',
        //   timecreated: '1557059677',
        //   timemodified: '1557062764',
        //   forcetheme: '',
        //   enablecompletion: '0',
        //   completionnotify: '0',
        //   courseformatoptions: [
        //     {
        //       name: 'numsections',
        //       value: 1
        //     }
        //   ],
        //   cours_name: 'compilar17',
        //   have_exam: '0',
        //   Practical_mark: '130'
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
