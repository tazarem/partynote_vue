<template>
      <v-layout fill-height column :class="this.note.color" style="position:static !important;
      ">
<!-- 드로어 부분! -->
    <v-navigation-drawer
      v-model="RightDrawer"
      app
      clipped
      right
    >
      <v-list dense class="py-0" height="100%">
        <v-list-item class="black" style="opacity:0.6;">
          <!-- <v-list-item-action>
            <v-icon>mdi-cupcake</v-icon>
          </v-list-item-action> -->
          <v-list-item-content>
            <v-list-item-title class="d-flex flex-center justify-space-around align-center">
              <v-icon
              dark
              @click=""
              >mdi-chevron-left</v-icon>

              <v-chip dark>
                <!-- color="primary"  -->
                <v-icon small class="mr-2">mdi-eye</v-icon>
                <small>카드로 보기</small>
                <!-- 카드로 보기와 글로 보기가 있음 -->
                </v-chip>
              <v-icon
              dark
              @click=""
              >mdi-chevron-right</v-icon>
              </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
<!-- 드로워 컴포넌트 -->
    <v-btn-toggle v-model="SideToggle" dense tile group color="teal accent-5">
          <v-btn value="wizet">
          Widzet
          </v-btn>
          <v-btn value="category">
          List
          </v-btn>
          </v-btn-toggle>
          <v-layout column :class="SideToggle=='category'? 'scroll2':'scroll'" style="max-height:calc(100% - 92px);">
          <template v-if="SideToggle=='wizet'">
          <v-btn color="secondary" tile dark
          @click=""
          >
          <v-icon>mdi-plus</v-icon> 위젯 추가..
          </v-btn>
          <template>
            <!-- 여기다가 사용자가 위젯을 추가해야함. -->
          </template>
          <!-- <scheduler></scheduler>
        <v-divider></v-divider>
          <hotline></hotline>
        <v-divider></v-divider>
          <tagfilter></tagfilter>
        <v-divider></v-divider> -->
          </template>

          <template v-else>
            <template v-for="item in this.cards">
              <v-card :key="item.postIndex" :color="item.postColor" @click="showModal(item)" tile>
                <v-card-title>{{item.postTitle}}</v-card-title>
                <!-- <v-card-subtitle>{{item.postSubtitle}}</v-card-subtitle> -->
                <!-- <v-card-text>{{filterContents(item)}}</v-card-text> -->
              </v-card>
            </template>
            <v-btn fab absolute small bottom right class="mb-6 black" style="opacity: 0.57; z-index:5;">
              <v-icon color="white">mdi-chevron-up</v-icon>
            </v-btn>
          </template>

          </v-layout>
<!-- 드로워 컴포넌트 -->
      </v-list>
    </v-navigation-drawer>

          <!-- 보드타이틀 툴바 -->
    <!-- <v-layout style="max-height:fit-content; position:fixed;"> -->
      <v-toolbar class="white--text black selophan" dense flat
      style="position:fixed; width:100%; z-index:2;"
      >
        <span>{{NoteTitle}}</span>
        <v-spacer></v-spacer>
        <v-btn dark text fab x-small
        ><v-icon>mdi-menu-up</v-icon></v-btn>
      </v-toolbar>
      <!-- </v-layout> -->
<!-- 드로어 툴바 -->
      <v-toolbar color="transparent" flat absolute max-height="fit-content"
      style="top:48px; width:100%;">
      <!--  position:fixed; z-index:10;-->
      {{NoteDes}}
      <v-spacer></v-spacer>
      <v-tooltip bottom>
        <template v-slot:activator="{ on }">
        <v-btn fab small dark depressed class="mr-2"
        v-on="on"
        :style="toggleMode==='book'? 'opacity:1;':'opacity:0.57;'"
        :color="toggleMode==='book'? 'green':'black'"
        @click="toggleMode==='book'? toggleMode='default':toggleMode='book'"
        >
          <v-icon>mdi-book-plus-multiple</v-icon>
        </v-btn>
        </template>
        책 만들기..
      </v-tooltip>

       <v-tooltip bottom>
        <template v-slot:activator="{ on }">
        <v-btn fab small dark depressed class="mr-2"
        v-on="on"
        :style="toggleMode==='edit'? 'opacity:1;':'opacity:0.57;'"
        :color="toggleMode==='edit'? 'warning':'black'"
        @click="toggleMode==='edit'? toggleMode='default':toggleMode='edit'"
        >
          <v-icon>mdi-pencil</v-icon>
        </v-btn>
        </template>
        글 수정하기..
       </v-tooltip>
       <v-tooltip bottom>
        <template v-slot:activator="{ on }">
        <v-btn fab small dark depressed class="mr-2"
        v-on="on"
        :style="toggleMode==='delete'? 'opacity:1;':'opacity:0.57;'"
        :color="toggleMode==='delete'? 'error':'black'"
        @click="toggleMode==='delete'?toggleMode='default':toggleMode='delete'"
        >
          <v-icon>mdi-delete</v-icon>
        </v-btn>
        </template>
        글 삭제
       </v-tooltip>

        <v-btn fab small dark depressed color="black"
        style="opacity:0.57;"
        @click="drawerOpen"
        >
          <v-icon>{{RightDrawer===true?'mdi-chevron-right':'mdi-chevron-left'}}</v-icon>
        </v-btn>

      <!-- 삭제/수정 버튼 툴바란 -->
      </v-toolbar>

    <v-layout class="pl-3" style="max-height: calc(100% - 112px); position: absolute; top:112px;">
    <v-layout style="flex-wrap: wrap; min-width:190px; max-width:190px; max-height:100px;">
      <!--카드 생성 버튼 -->
      <v-hover
      v-slot:default="{ hover }"
      >
      <v-card class="pa-3 ml-4 mb-2 defaultMode"
      min-width="fit-content"
      max-height="50"
      color="black" dark flat
      v-if="true"
      :elevation="hover? 10:3"
      style="opacity:0.6;"
      @click="openCreateModal"
      >
      <span class="mr-3">새 포스트..</span>
        <v-icon>mdi-plus</v-icon>
      </v-card>
      </v-hover>
      <!-- 카드 생성 버튼 끝 -->
    </v-layout>

<!--     max-height: 793px;
100%- 176
    overflow: auto;calc(100% - 176px); -->
      <v-layout class="pl-0 scroll"
      style="flex-wrap: wrap !important; align-content: flex-start;
      max-height: 100%;
      justify-content: flex-start;">
          <!-- 신규 생성되는 구간 -->
      <template v-for="(book,index) in this.books">
        <v-card :key="book.bookCode"
        class="px-3 pt-3 pb-0 mr-3 mb-2"
        width="225"
        min-height="138"
        max-height="138"
        :color="`${book.posts[0].postColor}`"
        :id="book.bookCode+index"
        @dragend="cardDragEnd"
        @dragover="allowCardDrop"
        @drop="dropCard"
        @click="openBook(book)"
        >
        <span class="white--text">{{book.bookTitle}}</span>
        <v-divider></v-divider>
        <small style="position:absolute; right:10px; bottom:10px;" class="white--text">
          <span>{{book.bookPage}}page</span>
          <v-icon color="white">mdi-book</v-icon>
        </small>
        </v-card>
      </template>
      <template class="card-zone"
      v-for="(card , index) in this.cards"
      >

      <!--떠오르는 이펙트를 위한 v-hover -->
      <v-hover
      v-slot:default="{ hover }"
      :key="index"
      >
      <!--떠오르는 이펙트를 위한 v-hover -->

      <v-card
      class="pa-3 mr-3 mb-2 card-posts"
      :class="nowModes()"
      width="225"
      min-height="138"
      max-height="138"
      style="overflow:hidden;"
      height="fit-content"
      :color="card.postColor"
      flat
      absolute
      :elevation="hover? 10:3"
      :id="card.postCode+index"
      :key="card.postCode"
      @click="toggleMode==='default'?showModal(card):editOrDelete(card)"
      @dragstart="cardDragStart"
      @dragend="cardDragEnd"
      @dragover="allowCardDrop"
      @drop="dropCard"
      :draggable="toggleMode==='default'||toggleMode==='book'? true:false"
      >
      <!--
      max-width="300"
      max-height="100" -->
        <!-- 카드 -->
        <template>
        <span
        >{{ card.postTitle }}</span>
        <!-- .substring(0,15) -->
        <!-- <small v-if="card.postTitle.length>15">..</small> -->
        <v-divider></v-divider>
        <div class="d-flex justify-end align-center my-1">
                <span style="font-size:13px;"
                class="font-weight-bold">{{card.postSubtitle.substring(0,11)}}</span>
                <!-- .substring(0,10) -->
                <v-spacer></v-spacer>
                <small class="overline"></small>
                <!-- small에는 날짜가 -->
        </div>
        <small class="body-2" v-html="filterContents(card)">
          <!--  .replace(/>[^>]*>?/gm).substring(0 ,64)-->
        </small>
        </template>
        <!-- 카드 -->

      </v-card>
      </v-hover>
      </template>
      <!-- 신규 생성되는 구간 -->
</v-layout>
              <!-- 모달 다이얼로그 컴포넌트 -->
      <modal
      v-on:newcard="createNewCard"
      v-on:close="closeModal"
      v-on:doedit="doEdit"
      v-on:newbook="makingBook"
      v-on:deletebook="deleteBook"
      v-on:releasebook="releaseBook"
      :book="showBook"
      :bookTitle="showBookTitle"
      :editCard="editCard"
      :modalCase="ModalCase"
      :dialog="OpenModal"
      v-if="OpenModal"
      >
      <!-- 여기부분을 수정/삭제에 맞춰서! -->
      <!-- 수정일 경우 create 폼과 같이 설정하지만 그 안에 있는 요소를 전달해줘야 함  -->
        <template v-slot:header>{{ModalShow.title}}</template>
        <template v-slot:recentdate>{{ModalShow.date}}</template>
        <template v-slot:subtitle>{{ModalShow.subtitle}}</template>
        <template v-slot:contents >
          <div v-html="ModalShow.contents"></div>
        </template>
      </modal>
      <!-- 모달 다이얼로그 컴포넌트 -->

    </v-layout>
</v-layout>
</template>

<script>
import Modal from '../components/modal.vue'
import axios from 'axios'
export default {
  components: {
    Modal
  },
  created () {
    // 유저 정보 불러오기. axios를 이용하여 다이어리와 포스트를 불러와 백엔드와 통신.
    this.bringPosts()
    this.bringBooks()
    this.note.color = sessionStorage.getItem('anColor')
    // this.user.icon = this.randomAvatar()
  },

  data () {
    return {
      SideToggle: 'wizet',
      note: {
        color: 'grey',
        icon: 'mdi-cupcake'
      },
      cards: [],
      books: [],
      showBookTitle: '',
      showBook: [],
      PagesToCreateBook: {},
      thisNoteCode: this.$route.params.noteCode,
      NoteTitle: sessionStorage.getItem('anTitle'),
      NoteDes: sessionStorage.getItem('anDes'),
      toggleMode: 'default',
      NewDiaryBtn: false,
      OpenModal: false,
      RightDrawer: false,
      ModalShow: {
        title: '',
        date: '',
        color: '',
        contents: '',
        button: ''
      },
      editCard: {},
      ModalCase: '',
      EditedTitle: 'New Card'
    }
  },
  watch: {
    SideToggle: function () {

    }
  },
  methods: {
    nowModes () {
      const mode = this.toggleMode
      switch (mode) {
        case 'edit': {
          return 'editMode' }
        case 'delete': {
          return 'deleteMode' }
        case 'book': {
          return 'bookMode' }
        default: {
          return 'defaultMode' }
      }
    },
    editOrDelete (card) {
      const mode = this.toggleMode
      switch (mode) {
        case 'edit': {
          this.ModalShow.title = '수정하기'
          this.ModalCase = 'edit'
          this.editCard = card
          this.OpenModal = true
          console.log('edit this : ' + card.postCode) // 노트의 이름과 색, 디스크립션을 수정할 수 있도록 하기.
          // 모달창 열리고 확인버튼 받아야지 아이작스 요청!
          break }
        case 'delete': {
          const answer = confirm('정말 이 카드를 삭제할까요?')
          if (answer) {
            console.log('delete this : ' + card.postCode) // 노트를 삭제. disable 처리하기. (하위 항목은 전부 지울까?)
            axios.post('/partynote/deletePost', card).then((req) => {
              console.log('delete : ' + req)
              // 다시 불러오기
              this.bringPosts()
            })
          }

          break }
      }
    },
    doEdit (card) {
      console.log(card)
      axios.post('/partynote/editPost', card).then((req) => {
        this.bringPosts()
      })
    },
    drawerOpen () {
      this.RightDrawer = !this.RightDrawer
      this.$emit('drawerControll')
    },
    openCreateModal () {
      this.ModalCase = 'create'
      this.ModalShow = {
        title: '새 카드 만들기',
        date: '',
        contents: ''
      }
      // console.log(this.ModalShow)
      this.OpenModal = true // 오버레이 컴포넌트 만들어야하고
    },
    createNewCard (newCard) {
      this.OpenModal = false
      newCard.postCode = this.thisNoteCode
      newCard.postColor = this.randomColor()
      newCard.date = this.bringDate()
      newCard.postIndex = this.cards.length
      console.log(newCard)
      // 최근 것의 인덱스를 가져와서 +1 을 해서 속성부여한다.
      axios.post('/partynote/makePost', newCard).then((req) => {
        this.bringPosts()
      })
      // this.cards.splice(0, 0, newCard) // 최신 포스트가 앞에 들어감
    },
    bringDate () {
      let nowAdays = ''
      const date = new Date()
      const d = {
        yyyy: date.getFullYear(),
        mm: date.getMonth(),
        dd: date.getDay(),
        hh: date.getHours(),
        md: date.getMinutes()
      }
      let amOrPm = ''
      // console.log(d.yyyy)
      const keyss = Object.keys(d)
      // console.log(keyss)
      for (const key in keyss) {
        if (d[keyss[key]] < 10) {
          d[keyss[key]] = '0' + d[keyss[key]]
        }
        if (d[keyss[key]] > 12) {
          amOrPm = '오후'
        } else {
          amOrPm = '오전'
        }
      }
      nowAdays = `${d.yyyy}-${d.mm}-${d.dd} ${amOrPm} ${d.hh}:${d.mm}`
      return nowAdays
    },
    closeModal () {
      // 값 초기화
      this.ModalShow = {
        title: '',
        date: '',
        subtitle: '',
        contents: ''
      }
      this.ModalCase = 'create'
      // 모달 비활성화
      this.OpenModal = false
    },
    showModal (post) {
      // console.log(post)
      this.ModalShow = {
        title: post.postTitle,
        date: post.date,
        color: post.postColor,
        subtitle: post.postSubtitle,
        contents: post.postContents
      }
      this.ModalCase = 'show'
      // 모달 활성화
      this.OpenModal = true
    },
    // 드래그앤 드랍 개체의 함수
    cardDragStart (e) {
      const objid = this.deepSet(e.target.id) // index
      e.dataTransfer.setData('text', objid)
      setTimeout(() => {
        e.target.style.opacity = 0
      }, 0)
    },
    cardDragEnd (e) {
      setTimeout(() => {
        e.target.style.opacity = 1
      }, 0)
    },
    deepSet (obj) { // 포인터때문에 json 변환 ㅡㅡ;;
      return JSON.parse(JSON.stringify(obj))
    },
    // 드래그앤 드랍 존
    allowCardDrop (e) {
      e.preventDefault()
    },
    async dropCard (e) {
      // splice(0,this.thisNoteCode.length+5) 를 제거하면 인덱스가 남음. 노트코드를 제거한 5글자
      const a = (this.deepSet(e.target.id))
      const b = (this.deepSet(e.dataTransfer.getData('text')))

      const zoneIsBook = (a.substring(this.thisNoteCode.length + 1, this.thisNoteCode.length + 2) === 'b')
      const dragIsBook = (b.substring(this.thisNoteCode.length + 1, this.thisNoteCode.length + 2) === 'b')

      const zoneIndex = a.substring(this.thisNoteCode.length + 5, this.thisNoteCode.length + 6)
      const sortIndex = b.substring(this.thisNoteCode.length + 5, this.thisNoteCode.length + 6) // zone인덱스만 남고
      console.log(zoneIndex)
      console.log(sortIndex)
      if (this.toggleMode === 'book') {
        if (zoneIsBook) {
          console.log('add Book Page')
          this.addBookPage(b.substring(0, this.thisNoteCode.length + 5), a.substring(0, this.thisNoteCode.length + 5))
        } else if (zoneIsBook === false && dragIsBook === false) { // 둘다 카드
          console.log('making book')
          this.openBookModal(zoneIndex, sortIndex)
        }
      } else { // 단순 순서 바꾸기
        const temp = this.deepSet(this.cards[sortIndex])
        let newCardsArray = this.deepSet(this.cards)
        await newCardsArray.splice(sortIndex, 1, this.cards[zoneIndex])
        await newCardsArray.splice(zoneIndex, 1, temp)
        newCardsArray = await this.indexSetting(newCardsArray, sortIndex, zoneIndex)
        this.cards = newCardsArray
      }
      // 이 두 개체를 가지고 업데이트
      // axios.post(map 형태로 이루어진 인덱스 배열 넘겨서 업데이트 치기)
    },
    indexSetting (newArray, sort, zone) {
      newArray[sort].postIndex = sort
      newArray[zone].postIndex = zone
      return newArray
    },
    openBookModal (firstPage, secondPage) {
      this.ModalCase = 'makebook'
      this.ModalShow.title = '책 생성'
      this.PagesToCreateBook = {
        fp: firstPage,
        sp: secondPage
      }
      this.OpenModal = true
    },
    openBook (bookItem) {
      this.showBook = bookItem
      this.ModalCase = 'book'
      this.showBookTitle = bookItem.bookTitle
      this.OpenModal = true
    },
    makingBook (bt) {
      // 제목설정 모달창 띄워주고 텍스트 입력하고 확인 누르면
      const fp = this.cards[this.PagesToCreateBook.fp]
      const sp = this.cards[this.PagesToCreateBook.sp]
      if (fp == null || sp == null) {

      } else {
        axios.post('/partynote/makeBook', {
          posts: [fp,
            sp],
          bookTitle: bt,
          bookPage: 2

        }).then((res) => {
          console.log(res)
          this.OpenModal = false
          this.bringPosts()
          this.bringBooks()
        })
      }
      // axios 요청
      // bookcard 객체 만들어서 넘기기
    },
    addBookPage (pCode, bCode) {
      console.log('add Page:' + pCode)
      axios.post('/partynote/addPageToBook', {
        postCode: pCode,
        bookCode: bCode
      }).then((res) => {
        this.bringPosts()
        this.bringBooks()
      })
    },
    deleteBook (book) {
      console.log(`delete this : ${book.bookCode}`)
      axios.post('/partynote/deleteBook', book).then((res) => {
        this.OpenModal = false
        this.bringPosts()
        this.bringBooks()
      })
    },
    releaseBook (book) {
      axios.post('/partynote/releaseBook', book).then((res) => {
        this.OpenModal = false
        this.bringPosts()
        this.bringBooks()
      })
    },
    filterContents (card) {
      const deleteTags = card.postContents.replace(/(<([^>]+)>)/ig, '')
      let textThumbnail = deleteTags.substring(0, 47)
      if (textThumbnail.length === 47) {
        textThumbnail += '..'
      }
      return textThumbnail
    },
    // 탑바 계정 랜덤 결정기
    randomAvatar () {
      const str = 'mdi-'
      const icons = [
        'cupcake',
        'cup-water',
        'cat',
        'dog',
        'cow',
        'fruit-watermelon',
        'fruit-pineapple',
        'fishbowl',
        'castle',
        'chef-hat',
        'skull'
      ]
      const randomCase = Math.floor(Math.random() * 11)
      return `${str}${icons[randomCase]}`
    },
    randomColor () {
      const colors = [
        'red',
        'pink',
        'purple',
        'deep-purple',
        'indigo',
        'blue',
        'light-blue',
        'cyan',
        'teal',
        'green',
        'light-green',
        'lime',
        'yellow',
        'amber',
        'orange',
        'deep-orange'
      ]
      const brighten = ['lighten-3', 'lighten-2', 'lighten-1']
      const colorCase = Math.floor(Math.random() * 16)
      const brightCase = Math.floor(Math.random() * 3)

      return `${colors[colorCase]} ${brighten[brightCase]}`
    },
    bringPosts () {
      axios.post('/partynote/bringPost', this.thisNoteCode).then((res) => {
        console.log(res.data)
        this.cards = res.data
      })
    },
    bringBooks () {
      axios.post('/partynote/bringBooks', this.thisNoteCode).then((res) => {
        console.log('bringBooks')
        console.log(res.data)
        this.books = res.data
      })
    }
  },

  beforeDestroy () {
    // 포스트인덱스 새로 for로 업데이트 쳐서 인덱스 axios 시켜야함
    const cards = this.cards
    for (const i in cards) {
      cards[i].postIndex = i
    }
    if (cards.length > 0) {
      axios.post('/partynote/updatePostIndex', cards).then((res) => {
        console.log(res.data.answer)
      })
    }
  }

}
</script>

<style>

</style>
