<script lang="ts">
  import { Router, Link, Route } from "svelte-routing";
  import Login from "./routes/Login.svelte";
  import TeacherHome from "./routes/TeacherHome.svelte";
  import StudentHome from "./routes/StudentHome.svelte";
</script>

<Router>
  <main>
    <Route path="/" component={Login} />
    <Route path="/teacher" component={TeacherHome} />
    <Route path="/student" component={StudentHome} />
  </main>
</Router>