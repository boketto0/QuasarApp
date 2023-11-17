<template>
  <q-page class="row items-center justify-evenly">
    <notes-list
      title=""
      :active="true"
      :todos="todos"
      :meta="meta"
      @addNote="addNote"
      @removeNote="removeNote"
    ></notes-list>
  </q-page>
</template>

<script lang="ts">
import { Todo, Meta } from 'components/models';
import NotesList from 'components/NotesList.vue';
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'IndexPage',
  components: { NotesList },
  setup () {
    const todos = ref<Todo[]>([]);
    const meta = ref<Meta>({
      totalCount: 1200
    });

    function addNote(newNote: Todo) {
      todos.value.push({
        id: todos.value.length + 1,
        content: newNote.content
      });
    }

    function removeNote(id: number) {
      todos.value = todos.value.filter(todo => todo.id !== id);
    }

    return { todos, meta, addNote, removeNote };
  }
});
</script>
