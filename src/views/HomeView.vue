<template>
  <div class="Main-container">
    <div class="Data-Container">
      <div class="Student-List">
        <div class="Student-Item" v-for="student in studentList" :key="student.id">
          <div class="Avatar-Box">
            <template v-for="(avatar, index) in student.avatar">
              <div class="Avatar" :id="index" @click="changeStudent(student.id, index)">
                <img :key="index" :src="avatar" draggable="false" />
              </div>
            </template>
          </div>
          <div class="Name">{{ student.name }}</div>
        </div>
      </div>
    </div>

    <div class="Talk-Container">
      <div class="Talk"></div>
      <div class="Edit">
        <div class="Input-Box">
          <button class="Input-Avatar">
            <div class="Avatar">
              <img :src="'/HeadIcon/student/sensei.webp'" />
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

          <div class="Edit-Item" :id="index" v-for="(student, index) in talkList" :key="index"
            :style="{ width: '100px', 'justify-content': 'space-around' }">
            <div class="Avatar">
              <img :src="studentList[student.id]['avatar'][student.avatar]" draggable="false" />
            </div>

            <svg focusable="false" aria-hidden="true" viewBox="0 0 24 24" data-testid="CancelIcon">
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
      message: '',
      newStudent: '',
      studentList: [],
      talkList: [
        {
          id: 0,
          avatar: 0,
        }
      ],
    }
  },
  methods: {
    sendMessage() {
      console.log(this.message);
    },
    changeStudent(id, index) {
      const newSrc = this.studentList[id]["avatar"][index];
      const avatarImg = document.querySelector('.Input-Avatar .Avatar img');
      avatarImg.src = newSrc;
      this.message = '';

      var adder = {
        id: id,
        avatar: index
      }

      const hasDuplicate = this.talkList.some(item => {
        return item.id === adder.id && item.avatar === adder.avatar;
      });

      if (!hasDuplicate) {
        this.talkList.push(adder);
      }
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
  width: 69.5%;
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
    background-color: #fff7e1;
  }

  .Edit {
    width: 100%;
    height: 25%;
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
      width: 100%;
      height: auto;
      padding: 10px;

      .Edit-Item {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 60px;
        height: 60px;
        margin-right: 10px;
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
  }
}
</style>
