<template>
  <q-page class="bg-grey-3 column">
    <q-list class="bg-white"
      separator
      bordered>
      <q-item 
        clickable
        v-for="(task, index) in tasks" 
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1' : task.done}"
        v-ripple>
        <q-item-section avatar>
          <q-checkbox 
            v-model="task.done" 
            class="no-pointer-events"
            color="primary"
            />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section 
          v-if="task.done"
          side>
          <q-btn @click.stop="deleteTask(index)" flat round color="primary" icon="delete" />
        </q-item-section>
      </q-item>      
    </q-list>
    <div class="absolute-bottom">
        <q-btn round color="red" icon="add" @click="addTodo()" />
    </div>
  </q-page>
</template>

<script>
export default {
  data(){
    return {
      tasks: [
        {
          title: "Work on CSC209",
          done: false
        },
        {
          title: "Eat Bananas",
          done: true
        },
        {
          title: "Complete Q1",
          done: false
        }
      ]
    }
  },
  methods: {
    deleteTask: function(index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Really Delete',
        cancel: true,
        persistent: true
      }).onOk(() => {
       this.tasks.splice(index, 1);
       this.$q.notify('Task deleted!');
      })
    },
    addTodo: function(){
       this.$q.dialog({
        color: 'red',
        title: 'Add a Todo',
        message: 'What do you want to add to the list?',
        prompt: {
          label: 'Type Your Todo Here!',
          type: 'text'
        },
        cancel: true,
        persistent: true
      }).onOk(data => {
        if (data.length > 0){
          this.tasks.push({'title': data, 'done': false});
          this.$q.notify('Task Added!');
        }
      })
    }
  }
}
</script>

<style lang="scss">
  .done {
    .q-item__label {
      text-decoration: line-through;
      color: #bbbbbb;
    }
  }
</style>
