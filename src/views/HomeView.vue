<template>
  <div class="Main-container">
    <div class="Data-Container">
      <div class="Student-List">
        <div class="Student-Item" v-for="student in studentList.slice(1)" :key="student.id">
          <div class="Avatar-Box">
            <template v-for="(avatar, avatarIndex) in student.avatar">
              <div class="Avatar" :id="avatarIndex" @click="changeStudent(student.id, avatarIndex)">
                <img :key="avatarIndex" :src="avatar" draggable="false" />
              </div>
            </template>
          </div>
          <div class="Name">{{ student.name }}</div>
        </div>

      </div>
    </div>

    <div class="Talk-Container">
      <div class="Talk">

        <div class="Talk-Student">
          <div class="Talk-Left">
            <div class="Avatar">
              <img :src="studentList[nowStudent.id]['avatar'][nowStudent.avatar]" draggable="false" />
            </div>
          </div>
          <div class="Talk-Right">
            <div class="Talk-Name"><strong>{{ studentList[nowStudent.id].name }}</strong></div>
            <div class="Talk-Message">
              {{ message }}
            </div>
          </div>
        </div>

        <div class="Talk-Sensei">
          <div class="Talk-Message">
            {{ message }}
          </div>
        </div>

      </div>
      <div class="Edit">
        <div class="Input-Box">
          <button class="Input-Avatar">
            <div class="Avatar">
              <img :src="studentList[nowStudent.id]['avatar'][nowStudent.avatar]" draggable="false" />
            </div>
          </button>

          <div class="Input-Text">
            <v-textarea class="input" label="聊天内容" v-model="message" name="input" rows="2" variant="underlined"
              auto-grow />
          </div>

          <button class="Input-Send" @click="sendMessage">
            <svg focusable="false" aria-hidden="true" viewBox="0 0 24 24" data-testid="SendIcon" fill="#757575">
              <path d="M2.01 21 23 12 2.01 3 2 10l15 2-15 2z"></path>
            </svg>
          </button>
        </div>

        <div class="Edit-Item-Box">
          <div class="Edit-Item">
            <div class="Avatar">
              <img :src="'/HeadIcon/student/sensei.webp'" draggable="false" />
            </div>
          </div>

          <div class="Edit-Item" :id="index" v-for="(student, index) in talkedStudentList" :key="index"
            :style="{ width: '100px', 'justify-content': 'space-around' }"
            @click="changeStudent(student.id, student.avatar)">
            <div class="Avatar">
              <img :src="studentList[student.id]['avatar'][student.avatar]" draggable="false" />
            </div>

            <svg focusable="false" aria-hidden="true" viewBox="0 0 24 24" data-testid="CancelIcon"
              @click="deleteItem(index)">
              <path
                d="M12 2C6.47 2 2 6.47 2 12s4.47 10 10 10 10-4.47 10-10S17.53 2 12 2zm5 13.59L15.59 17 12 13.41 8.41 17 7 15.59 10.59 12 7 8.41 8.41 7 12 10.59 15.59 7 17 8.41 13.41 12 17 15.59z">
              </path>
            </svg>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
import studentData from '@/assets/student.json';
export default {
  data() {
    return {
      message: 'asdasdasdasd',
      preStudent: {
      },
      nowStudent: {
        id: 0,
        avatar: 0
      },
      studentList: [],
      talkedStudentList: [],
      dataItem: {
        is_breaking: false,
        studentId: 0,
        name: '',
        avatarId: 0,
        avatarState: 'Show',
        type: 'TEXT',
        content: '',
      },
      dataList: []
    }
  },
  methods: {
    sendMessage() {
    },
    changeStudent(id, index) {
      if (!this.preStudent.id) {
        this.preStudent.id = this.nowStudent.id;
        this.preStudent.avatar = this.nowStudent.avatar;
      };

      this.preStudent.id = this.nowStudent.id;
      this.preStudent.avatar = this.nowStudent.avatar;
      this.nowStudent.id = id;
      this.nowStudent.avatar = index;
      this.message = '';

      var adder = {
        id: id,
        avatar: index
      }

      const hasDuplicate = this.talkedStudentList.some(item => {
        return item.id === adder.id && item.avatar === adder.avatar;
      });

      if (!hasDuplicate) {
        this.talkedStudentList.push(adder);
      }
    },
    deleteItem(index) {
      this.nowStudent.id = this.preStudent.id;
      this.nowStudent.avatar = this.preStudent.avatar;
      this.talkedStudentList.splice(index, 1);
    }
  },
  beforeMount() {
    this.studentList = studentData;
  }
}
</script>

<style>
.Avatar {
  width: 70px;
  height: 70px;
  border-radius: 50%;
  overflow: hidden;
  cursor: pointer;
  user-select: none;

  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    user-select: none;
    image-rendering: -webkit-optimize-contrast;
  }
}

.Main-container {
  display: flex;
  width: 100vw;
  height: 100vh;
}

.Data-Container {
  width: 70%;
  height: 100vh;
  background-color: #dddddd;

  .Student-List {
    width: 100%;
    height: 100%;
    padding: 20px;
    overflow-y: auto;

    .Student-Item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding-block: 10px;
      font-size: 20px;
      border-bottom: 2px dashed #bbbbbb;

      .Avatar-Box {
        display: flex;

        .Avatar {
          margin-right: 10px;
        }
      }
    }
  }

}

.Talk-Container {
  width: 30%;
  height: 100vh;
  box-shadow: 3px 0 15px 5px rgba(0, 0, 0, 0.2);

  .Talk {
    width: 100%;
    height: 75%;
    z-index: 1;
    background-color: #fff7e1;

    .Talk-Student {
      display: flex;
      flex-direction: row;
      padding: 8px 24px 0 16px;

      .Talk-Left {
        display: flex;
        justify-content: center;
        align-items: center;
        margin-right: 16px;
        width: 70px;
        height: 70px;
      }

      .Talk-Right {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: flex-start;
        max-width: 75%;
        height: 100%;

        .Talk-Name {
          font-size: 20px;
          font-weight: 700;
          margin-bottom: 4px;
        }

        .Talk-Message {
          position: relative;
          width: auto;
          height: 100%;
          padding: 8px;
          font-size: 20px;
          line-height: 24px;
          color: white;
          border-radius: 10px;
          background-color: #4c5b70;

          &::before {
            content: '';
            position: absolute;
            top: 8px;
            left: -15px;
            z-index: 2;
            border-width: 10px;
            border-style: solid;
            border-color: transparent #4c5b70 transparent transparent;
          }
        }

        .no-before::before {
          display: none;
        }
      }
    }

    .Talk-Sensei {
      display: flex;
      flex-direction: row-reverse;
      padding: 8px 24px 0 16px;

      .Talk-Message {
        position: relative;
        max-width: 83%;
        height: 100%;
        padding: 8px;
        font-size: 20px;
        line-height: 24px;
        color: white;
        border-radius: 10px;
        background-color: #4a8aca;

        &::before {
          content: '';
          position: absolute;
          top: 8px;
          right: -15px;
          z-index: 2;
          border-width: 10px;
          border-style: solid;
          border-color: transparent transparent transparent #4a8aca;
        }
      }
    }
  }

  .Edit {
    width: calc(100% - 5px);
    height: 25%;
    overflow: auto;
    background-color: white;

    .Input-Box {
      display: flex;
      flex-direction: row;
      align-items: center;

      .Input-Avatar {
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 0;
        width: 80px;
        height: 80px;
        margin-inline: 10px;
        border: none;

        .Avatar {
          box-shadow: 3px 3px 10px 2px rgba(0, 0, 0, 0.2);
        }
      }

      .Input-Text {
        display: flex;
        align-items: center;
        width: calc(100% - 145px);

        .input {
          margin-top: 20px;
        }
      }

      .Input-Send {
        display: flex;
        background-color: transparent;
        justify-content: center;
        align-items: center;
        padding: 5px;
        width: 45px;
        height: 45px;
        border: none;
        cursor: pointer;
      }
    }

    .Edit-Item-Box {
      display: flex;
      flex-direction: row;
      flex-wrap: wrap;
      width: calc(100% - 5px);
      height: 50%;
      padding: 10px;
      overflow: auto;

      .Edit-Item {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 60px;
        height: 60px;
        margin-right: 10px;
        margin-bottom: 10px;
        border-radius: 10px;
        border: 1px solid #bdbdbd;

        .Avatar {
          width: 50px;
          height: 50px;
        }

        svg {
          width: 20px;
          fill: #bdbdbd;
        }

        svg:hover {
          width: 20px;
          fill: #939393;
        }
      }

      .Edit-Item:active {
        box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.4);
      }

      .Edit-Item:hover {
        background-color: #f5f5f5;
      }
    }

    .Edit-Item-Box::-webkit-scrollbar {
      width: 5px;
    }

    .Edit-Item-Box::-webkit-scrollbar-track {
      background: transparent;
    }

    .Edit-Item-Box::-webkit-scrollbar-thumb {
      background-color: rgba(0, 0, 0, 0.3);
      border-radius: 6px;
      border: 3px solid transparent;
    }

    .Edit-Item-Box::-webkit-scrollbar-thumb:hover {
      background-color: rgba(0, 0, 0, 0.5);
    }

  }

  .Edit::-webkit-scrollbar {
    width: 5px;
  }

  .Edit::-webkit-scrollbar-track {
    background: transparent;
    /* 设置滚动条轨道背景为透明 */
  }

  .Edit::-webkit-scrollbar-thumb {
    background-color: rgba(0, 0, 0, 0.3);
    /* 设置滚动条 thumb 的背景颜色为半透明黑色 */
    border-radius: 6px;
    border: 3px solid transparent;
  }

  .Edit::-webkit-scrollbar-thumb:hover {
    background-color: rgba(0, 0, 0, 0.5);
    /* 设置滚动条 thumb 悬停时的背景颜色为更加透明的黑色 */
  }
}
</style>
