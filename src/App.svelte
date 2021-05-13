<script>
    import { Route } from "tinro";
    import Search from "./Search.svelte";
    import Profile from "./Profile.svelte";

    import "./global.scss";
</script>

<main>
    <Route path="/">
        <Search />
    </Route>
    <Route path="/:profile" let:meta>
        <Profile user={meta.url} />
    </Route>
</main>

<style>
    main {
        margin: 0 auto;
        max-width: 450px;
    }
</style>
