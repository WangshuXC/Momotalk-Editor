<template>
  <div class="Body-Container">
    <div class="Nav-Bar">
      <button class="Reset" @click="this.dataList = [], saveToLocalStorage()">
        <svg class="icon icon reset" viewBox="0 0 1025 1024" version="1.1" xmlns="http://www.w3.org/2000/svg"
          p-id="2275">
          <path
            d="M591.963942 6.17051C409.548578-22.304204 233.4705 49.707042 121.688176 182.211672l-3.617789-81.387966a59.065957 59.065957 0 0 0-61.625482-56.4326A59.065957 59.065957 0 0 0 0.061527 106.016588l9.610523 216.365983a59.065957 59.065957 0 0 0 61.625482 56.383378l216.365983-9.610524a59.065957 59.065957 0 0 0 56.346462-61.613176 59.065957 59.065957 0 0 0-61.588566-56.420294l-66.793752 2.965603c84.242821-97.05275 213.941818-150.445914 349.067499-132.52924 227.908456 30.234387 382.341322 250.279687 333.193524 474.853376-45.160846 206.373992-246.243513 341.290482-455.607719 302.909915a393.071637 393.071637 0 0 1-278.680568-206.263243 58.942903 58.942903 0 0 0-78.237782-25.533721 59.065957 59.065957 0 0 0-26.395099 80.428145c70.411543 135.359484 200.319732 237.925057 362.012788 267.568784 274.287537 50.218369 537.561734-128.210043 593.600561-399.901138C1073.536071 329.765815 880.34117 51.183691 591.963942 6.17051z"
            p-id="2276"></path>
        </svg>
      </button>
      <button class="Download" @click="downloadImage">
        <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" class="icon download">
          <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
          <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g>
          <g id="SVGRepo_iconCarrier">
            <path
              d="M13 4H8.8C7.11984 4 6.27976 4 5.63803 4.32698C5.07354 4.6146 4.6146 5.07354 4.32698 5.63803C4 6.27976 4 7.11984 4 8.8V15.2C4 16.8802 4 17.7202 4.32698 18.362C4.6146 18.9265 5.07354 19.3854 5.63803 19.673C6.27976 20 7.11984 20 8.8 20H15.2C16.8802 20 17.7202 20 18.362 19.673C18.9265 19.3854 19.3854 18.9265 19.673 18.362C20 17.7202 20 16.8802 20 15.2V11"
              stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path>
            <path
              d="M4 16L8.29289 11.7071C8.68342 11.3166 9.31658 11.3166 9.70711 11.7071L13 15M13 15L15.7929 12.2071C16.1834 11.8166 16.8166 11.8166 17.2071 12.2071L20 15M13 15L15.25 17.25"
              stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path>
            <path d="M18 3V8M18 8L16 6M18 8L20 6" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            </path>
          </g>
        </svg>
      </button>

    </div>

    <div class="Main-container">

      <div class="Data-Container">
        <div class="Student-List" ref="studentList">
          <div class="Student-Item" v-for="student in displayedStudents" :key="student.id">
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
        <div class="Talk" ref="talk">

          <template v-for="( data, index ) in  dataList " :key="index">
            <div class="Talk-Student" v-if="data.studentId >= 4">
              <div class="Talk-Left">
                <div class="Avatar" v-if="data.avatarState !== 'Hide'">
                  <img :src="studentList[data.studentId]['avatar'][data.avatarId]" draggable="false" />
                </div>
              </div>
              <div class="Talk-Right">
                <div class="Talk-Name" v-show="data.avatarState !== 'Hide'"><strong>{{
        studentList[data.studentId].name
      }}</strong></div>
                <div class="Talk-Message" :class="{ 'no-before': data.avatarState === 'Hide' }">
                  <span contenteditable @input="updateContent(data, $event)">{{ data.content }}</span>
                </div>
              </div>
            </div>

            <div class="Talk-Sensei" v-else-if="data.studentId === 0">
              <div class="Talk-Message">
                <span contenteditable @input="updateContent(data, $event)">{{ data.content }}</span>
              </div>
            </div>

            <div class="Talk-Narration" v-else-if="data.studentId === 1">
              <div class="Talk-Message">
                <span contenteditable @input="updateContent(data, $event)">{{ data.content }}</span>
              </div>
            </div>

            <div class="Talk-Reply" v-else-if="data.studentId === 2">
              <div class="Reply-Box">
                <div class="Reply-Header">
                  <span class="text">回复</span>
                </div>

                <div class="Reply-Choices">
                  <div class="Reply-Choice" v-for="( item, Reply_index ) in  data.content.split('\n') "
                    :key="Reply_index">
                    <span class="text">
                      {{ item }}
                    </span>
                  </div>
                </div>

              </div>
            </div>

            <div class="Talk-Sutori" v-else-if="data.studentId === 3">
              <div class="Sutori-Box">
                <div class="Sutori-Header">
                  <span class="text">羁绊剧情</span>
                </div>
                <div class="Sutori-Choices">
                  <div class="Sutori-Choice">
                    <span class="text" contenteditable @input="updateContent(data, $event)">{{ data.content }}</span>
                  </div>
                </div>
              </div>
            </div>
          </template>

        </div>
        <div class="Edit">
          <div class="Input-Box">
            <button class="Input-Avatar">
              <div class="Avatar">
                <img :src="studentList[nowStudent.id]['avatar'][nowStudent.avatar]" draggable="false" />
              </div>
            </button>

            <div class="Input-Text">
              <v-textarea class="input" label="聊天内容" v-model="message" name="input" rows="2" max-rows="2"
                variant="underlined" auto-grow @keydown.enter="handleEnterKey" />
            </div>

            <button class="Input-Send" @click="sendMessage(), saveToLocalStorage()"
              :style="{ cursor: (this.message.trim() === '') ? 'not-allowed' : 'pointer' }">
              <svg focusable="false" aria-hidden="true" viewBox="0 0 24 24" data-testid="SendIcon" fill="#757575">
                <path d="M2.01 21 23 12 2.01 3 2 10l15 2-15 2z"></path>
              </svg>
            </button>
          </div>

          <div ref="editItemBox" class="Edit-Item-Box" @wheel="handleHorizontalScroll">
            <div class="Edit-Item-a" v-for="( item, index ) in  studentList.slice(0, 4) " :key="index"
              @click="change(item.id)">
              <v-tooltip activator="parent" location="top">{{ item.name }}</v-tooltip>
              <div class="Avatar">
                <img :src="item.avatar[0]" draggable="false" />
              </div>
            </div>

            <div class="Edit-Item" :id="index" v-for="(  student, index  ) in   talkedStudentList " :key="index"
              :style="{ 'justify-content': 'space-around' }" @click="changeStudent(student.id, student.avatar)">
              <v-tooltip activator="parent" location="top">{{ studentList[student.id].name }}</v-tooltip>
              <div class="Avatar">
                <img :src="studentList[student.id]['avatar'][student.avatar]" draggable="false" />
              </div>

              <svg focusable="false" aria-hidden="true" viewBox="0 0 24 24" data-testid="CancelIcon"
                @click="deleteItem(index); $event.stopPropagation()">
                <path
                  d="M12 2C6.47 2 2 6.47 2 12s4.47 10 10 10 10-4.47 10-10S17.53 2 12 2zm5 13.59L15.59 17 12 13.41 8.41 17 7 15.59 10.59 12 7 8.41 8.41 7 12 10.59 15.59 7 17 8.41 13.41 12 17 15.59z">
                </path>
              </svg>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import studentData from '@/assets/student.json';
import Image from '@/assets/Image.svg';
import Reset from '@/assets/Reset.svg';
import { domtoimage } from '@/assets/imgUtils/dom-to-image.js';
export default {
  data() {
    return {
      ImageSvg: Image,
      ResetSvg: Reset,
      message: '',
      preStudent: {
        id: 0,
        avatar: 0
      },
      nowStudent: {
        id: 0,
        avatar: 0
      },
      studentList: [],
      displayedStudents: [], // 当前展示的学生数据
      distanceToBottom: 50, // 距离底部的距离
      talkedStudentList: [
      ],
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
    handleEnterKey(event) {
      if (!event.shiftKey) {
        event.preventDefault();
        this.sendMessage();
        this.saveToLocalStorage();
      }
    },
    sendMessage() {
      if (this.message.trim() === '') {
        return;
      }
      this.dataItem = {
        is_breaking: false,
        studentId: this.nowStudent.id,
        name: this.studentList[this.nowStudent.id].name,
        avatarId: this.nowStudent.avatar,
        avatarState: (this.preStudent.id == this.nowStudent.id && this.preStudent.avatar == this.nowStudent.avatar) ? 'Hide' : 'Show',
        type: 'TEXT',
        content: this.message,
      }

      this.preStudent.id = this.nowStudent.id;
      this.preStudent.avatar = this.nowStudent.avatar;

      this.dataList.push(this.dataItem);
      this.message = '';
    },
    handleHorizontalScroll(event) {
      event.preventDefault();
      const container = this.$refs.editItemBox;
      container.scrollLeft += event.deltaY;
    },
    changeStudent(id, index) {
      this.nowStudent.id = id;
      this.nowStudent.avatar = index;

      var adder = {
        id: id,
        avatar: index
      }

      const hasDuplicate = this.talkedStudentList.some(item => {
        return item.id === adder.id && item.avatar === adder.avatar;
      });

      if (!hasDuplicate) {
        this.talkedStudentList.unshift(adder);
      }
    },
    change(id) {
      this.nowStudent.id = id;
      this.nowStudent.avatar = 0;
    },
    deleteItem(index) {
      this.talkedStudentList.splice(index, 1);
    },
    loadFromLocalStorage() {
      const dataList = localStorage.getItem('dataList');
      if (dataList) {
        this.dataList = JSON.parse(dataList);
      }

      const talkedStudentList = localStorage.getItem('talkedStudentList');
      if (talkedStudentList) {
        this.talkedStudentList = JSON.parse(talkedStudentList);
      }
    },
    saveToLocalStorage() {
      localStorage.setItem('talkedStudentList', JSON.stringify(this.talkedStudentList));
      localStorage.setItem('dataList', JSON.stringify(this.dataList));
    },
    updateContent(data, event) {
      data.content = event.target.textContent;
      this.saveToLocalStorage();
    },
    downloadImage() {
      const node = document.getElementsByClassName('Talk')[0];
      const width = node.clientWidth;
      const height = node.scrollHeight;
      if (width && height) {
        domtoimage
          .toPng(node, { width, height })
          .then(function (dataUrl) {
            const link = document.createElement('a')
            link.download = `Momotalk-${Date.now()}.png`
            link.href = dataUrl
            link.click()
          })
          .catch(function (error) {
            console.error('oops, screenshot went wrong!', error)
          })
      }
    },
    fetchInitialData() {
      // 获取初始数据并更新 studentList 和 displayedStudents
      this.displayedStudents = this.displayedStudents.concat(
        this.studentList.slice(4, 20)
      );
    },
    fetchMoreData() {
      console.log('fetch more data');
      this.displayedStudents = this.displayedStudents.concat(
        this.studentList.slice(this.displayedStudents.length + 4, this.displayedStudents.length + 20)
      );
    },
    setupScrollListener() {
      const studentList = this.$refs.studentList;
      studentList.addEventListener('scroll', () => {
        if (this.isNearBottom(studentList)) {
          this.fetchMoreData();
        }
      });
    },
    isNearBottom(container) {
      const scrollHeight = container.scrollHeight;
      const offsetHeight = container.offsetHeight;
      const scrollTop = container.scrollTop;
      return scrollHeight - (scrollTop + offsetHeight) < this.distanceToBottom;
    },
  },
  mounted() {
    this.loadFromLocalStorage();
    this.fetchInitialData();
    this.setupScrollListener();
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

.Body-Container {
  display: flex;
  width: 100vw;
  height: 100vh;

  .Nav-Bar {
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    width: 5vw;
    height: 100vh;
    background-color: #4c5b70;

    .Reset {
      display: flex;
      justify-content: center;
      align-items: center;
      width: 5vw;
      height: 5vw;
      background-color: #4c5b70;
      border: none;
      cursor: pointer;

      svg {
        width: 35px;
        fill: #a5adb7;
      }
    }

    .Reset:hover {
      background-color: #68788f;
      transition: .6s;

      svg {
        fill: #fff;
      }
    }

    .Download {
      display: flex;
      justify-content: center;
      align-items: center;
      width: 5vw;
      height: 5vw;
      background-color: #4c5b70;
      border: none;
      cursor: pointer;

      svg {
        width: 45px;
        fill: none;
        stroke: #a5adb7;
      }
    }

    .Download:hover {
      background-color: #68788f;
      transition: .6s;

      svg {
        stroke: #fff;
      }

    }
  }

  .Main-container {
    display: flex;
    min-width: 95vw;
    height: 100vh;
  }
}



.Data-Container {
  width: 70%;
  height: 100%;
  background-color: #dddddd;

  .Student-List {
    width: 100%;
    height: 100%;
    padding: 20px;
    display: flex;
    flex-direction: column;
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

      .Name {
        color: #4c5b70;
        font-size: 20px;
        font-weight: 550;
      }
    }
  }

}

.Talk-Container {
  display: flex;
  flex-direction: column;
  width: 30%;
  height: 100%;
  overflow: hidden;
  box-shadow: 3px 0 15px 5px rgba(0, 0, 0, 0.2);

  .Talk {
    width: 100%;
    height: calc(100% - 200px);
    /* min-height: 80vh; */
    padding-top: 8px;
    padding-bottom: 16px;
    overflow: auto;
    position: relative;
    background-color: #fff7e1;

    .Talk-Message {
      white-space: pre-wrap;

      span {
        box-sizing: border-box;
      }
    }

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
        max-height: 70px;
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
      padding: 16px 16px 0;

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

    .Talk-Narration {
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 12px;
      text-align: center;

      .Talk-Message {
        max-width: 90%;

        span {
          color: #4c5b70;
          font-size: 18px;
          font-weight: 700;
          white-space: pre-wrap;
        }
      }
    }

    .Talk-Reply {
      display: flex;
      flex-direction: row-reverse;
      padding: 16px 24px 0 16px;

      .Reply-Box {
        width: 70%;
        /* background-color: #f3f7f8; */
        background: url(/reply.png) no-repeat right top, #f3f7f8;
        border: 1px solid #d8d8d8;
        border-radius: 10px;
        overflow: hidden;
        padding: 16px;

        .Reply-Header {
          border-bottom: 1px solid #d8d8d8;
          padding-bottom: 8px;

          .text {
            border-left: 2px solid #3493f9;
            color: #4c5b70;
            font-size: 18px;
            font-weight: 700;
            padding: 0 8px;
          }
        }

        .Reply-Choices {
          margin-top: 8px;

          .Reply-Choice {
            background-color: #fff;
            border: 1px solid #e7ebec;
            border-radius: 5px;
            min-height: 42px;
            overflow-wrap: break-word;
            margin-bottom: 10px;
            padding: 8px;
            position: relative;
            text-align: center;
            white-space: pre-wrap;
            box-shadow: 0 2px 5px #0000003d;

            span {
              color: #4c5b70;
              font-size: 18px;
              font-weight: 700;
              line-height: 24px;
            }
          }
        }
      }
    }

    .Talk-Sutori {
      display: flex;
      flex-direction: row-reverse;
      padding: 16px 24px 0 16px;

      .Sutori-Box {
        width: 70%;
        background-color: #ffedf1;
        background: url(/sutori.png) no-repeat right top, #ffedf1;
        border: 1px solid #d8d8d8;
        border-radius: 10px;
        overflow: hidden;
        padding: 16px;


        .Sutori-Header {
          border-bottom: 1px solid #d8d8d8;
          padding-bottom: 8px;

          .text {
            border-left: 2px solid #ff92a4;
            color: #4c5b70;
            font-size: 18px;
            font-weight: 700;
            padding: 0 8px;
          }
        }

        .Sutori-Choices {
          margin-top: 8px;

          .Sutori-Choice {
            background-color: #ff92a4;
            border: 1px solid #ff92a4;
            border-radius: 5px;
            color: #4c5b70;
            display: block;
            min-height: 24px;
            padding: 8px 0;
            text-align: center;
            box-shadow: 0 2px 5px #0000003d;

            .text {
              color: #fff;
              font-size: 16px;
              font-weight: 700;
            }
          }
        }
      }
    }

    .Talk::-webkit-scrollbar {
      width: 5px;
    }

    .Talk::-webkit-scrollbar-track {
      background: transparent;
    }

    .Talk::-webkit-scrollbar-thumb {
      background-color: rgba(0, 0, 0, 0.3);
      border-radius: 6px;
      border: 3px solid transparent;
    }

    .Talk::-webkit-scrollbar-thumb:hover {
      background-color: rgba(0, 0, 0, 0.5);
    }
  }

  .Edit {
    width: calc(100% - 5px);
    padding-bottom: 20px;
    height: 200px;
    /* min-height: 30vh; */
    overflow: hidden;
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
        height: 100px;

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
      width: 100%;
      padding: 10px;
      overflow-x: hidden;
      overflow-y: hidden;
      white-space: nowrap;

      .Edit-Item {
        display: flex;
        justify-content: center;
        align-items: center;
        min-width: 120px;
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

      .Edit-Item-a {
        display: flex;
        justify-content: center;
        align-items: center;
        min-width: 60px;
        height: 60px;
        margin-right: 10px;
        margin-left: 10px;
        border-radius: 10px;
        border: 1px solid #bdbdbd;

        .Avatar {
          width: 50px;
          height: 50px;
        }
      }

      .Edit-Item-a:active {
        box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.4);
      }

      .Edit-Item-a:hover {
        background-color: #f5f5f5;
      }

      .Edit-Item:active {
        box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.4);
      }

      .Edit-Item:hover {
        background-color: #f5f5f5;
      }
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
</style>@/assets/imgUtils/dom-to-image.js
