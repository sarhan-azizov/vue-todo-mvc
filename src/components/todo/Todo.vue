<template src="./todo.html"/>
<style lang="scss" src="./todo.scss"/>

<script>
export default {
  name: 'Todo',
  data() {
    return {
      filter: 'all',
      todos: [],
    };
  },
  created() {
    const initialValue = window.localStorage.getItem('@VueTodoMVC');

    if (initialValue) {
      try {
        const { filter, todos } = JSON.parse(initialValue);
        this.todos = todos;
        this.filter = filter;
      } catch (e) {
        console.error(e);
      }
    }
  },
  watch: {
    $data: {
      handler(val) {
        window.localStorage.setItem('@VueTodoMVC', JSON.stringify(val));
      },
      deep: true,
    },
  },
  computed: {
    filteredTodos() {
      switch (this.filter) {
        case 'completed': return this.todos.filter((todo) => todo.completed);
        case 'active': return this.todos.filter((todo) => !todo.completed);
        default: return this.todos;
      }
    },
    todoAmount() {
      return this.filteredTodos.length;
    },
    completedTodos() {
      return this.todos.filter((todo) => todo.completed);
    },
  },
  methods: {
    handleDeleteCompletedTodos() {
      this.todos = this.todos.filter((todo) => !todo.completed);
    },
    handleAddTodo(e) {
      const todoName = e.target.value;

      if (todoName) {
        this.todos.push({
          id: Date.now(),
          name: todoName,
          completed: false,
        });
      }

      e.target.value = '';
      e.target.focus();
    },
    handleFilterTodos(type) {
      this.filter = type;
    },
    handleDeleteTodo(id) {
      this.todos = this.todos.filter((todo) => todo.id !== id);
    },
    handleToggleCompleteTodo(id) {
      this.todos = this.todos.map((todo) => {
        if (todo.id === id) {
          return ({
            ...todo, completed: !todo.completed,
          });
        }

        return todo;
      });
    },
  },
};
</script>
