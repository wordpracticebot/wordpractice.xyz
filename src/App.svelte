<script lang="ts">
    import Router from "svelte-spa-router";
    import { Toaster } from "svelte-french-toast";

    import Landing from "./components/Landing.svelte";
    import Team from "./components/Team.svelte";

    import Navbar from "./components/Navbar.svelte";
    import Footer from "./components/Footer.svelte";
    import Premium from "./components/Premium.svelte";
    import Subscriptions from "./components/Subscriptions.svelte";

    const routes = {
        "/": Landing,
        "/team": Team,
        "/premium": Premium,
        "/subscriptions": Subscriptions,
    };
    const urlParams = new URLSearchParams(window.location.search);
    const code = urlParams.get("code");

    if (code) {
        const apiUrl = import.meta.env.VITE_API_URL;

        fetch(`${apiUrl}/token?code=${code}`)
            .then((res) => res.json())
            .then((data) => {
                if (!data.token) return;

                localStorage.setItem("token", data.token);
                window.location.href = "/";
            });
    }
</script>

<Toaster />

<div class="m-auto w-11/12 sm:w-5/6 lg:w-3/4 grow">
    <Navbar />

    <main>
        <Router {routes} />
    </main>
</div>

<Footer />

<style global>
    @tailwind utilities;
    @tailwind components;
    @tailwind base;
</style>
