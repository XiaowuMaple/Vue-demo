<template>
  <div id="notes-list">

    <div id="list-header">
      <h2>Notes | coligo</h2>
      <div class="btn-group btn-group-justified" role="group">
        <!-- All Notes button -->
        <div class="btn-group" role="group">
          <button type="button" class="btn btn-default"
            @click="show = 'all'"
            :class="{active: show === 'all'}">
            All Notes
          </button>
        </div>
        <!-- Favorites Button -->
        <div class="btn-group" role="group">
          <button type="button" class="btn btn-default"
            @click="show = 'favorites'"
            :class="{active: show === 'favorites'}">
            Favorites
          </button>
        </div>
      </div>
    </div>
    <!-- render notes in a list -->
    <div class="container">
      <div class="list-group">
        <a v-for="(note, index) in filteredNotes"
          :key="index"
          class="list-group-item" href="javascript:void(0);"
          :class="{active: activeNote === note}"
          @click="updateActiveNote(note)">
          <h4 class="list-group-item-heading">
            {{note.text.trim().substring(0, 30)}}
          </h4>
        </a>
      </div>
    </div>

  </div>
</template>

<script>
import { mapState, mapGetters, mapActions } from 'vuex'
export default {
  data () {
    return {
      show: 'all'
    }
  },
  methods: {
    ...mapActions(['updateActiveNote'])
  },
  computed: {
    ...mapState({
      notes: ({note}) => note.notes,
      activeNote: ({note}) => note.activeNote
    }),
    filteredNotes() {
      const notes = this.notes
      return this.show === 'all' ? notes : notes.filter(note => note.favorite)
    }
  },
  /*computed: mapState({
    // 箭头函数，当只有一个参数是，可省略();当只有一条语句是，可以省略return
    notes: state => state.note.notes,
    activeNote: state => state.note.activeNote,
    filteredNotes() {
      if(this.show === 'all') {
        return this.notes
      } else if(this.show === 'favorites') {
        return this.notes.filter(note => note.favorite)
      }
    }
  }),*/
  /*computed: {
    // notes() {return this.$store.getters.notes},
    // activeNote() {return this.$store.getters.activeNote},

    ...mapGetters([
        'notes', 'activeNote'
      ]),
    filteredNotes() {
      if(this.show === 'all') {
        return this.notes
      } else if(this.show === 'favorites') {
        return this.notes.filter(note => note.favorite)
      }
    }
  }*/
}
</script>

<style>
#notes-list {
  float: left;
  width: 300px;
  height: 100%;
  background-color: #F5F5F5;
  font-family: 'Raleway', sans-serif;
  font-weight: 400;
}
#list-header {
  padding: 5px 25px 25px 25px;
}
#list-header h2 {
  font-weight: 300;
  text-transform: uppercase;
  text-align: center;
  font-size: 22px;
  padding-bottom: 8px;
}
#notes-list .container {
  height: calc(100% - 137px);
  max-height: calc(100% - 137px);
  overflow: auto;
  width: 100%;
  padding: 0;
}
#notes-list .container .list-group-item {
  border: 0;
  border-radius: 0;
}
.list-group-item-heading {
  font-weight: 300;
  font-size: 15px;
}
</style>
