<script>
  import '../app.postcss'; // Ensures Tailwind styles are applied
</script>

<!-- Main Layout Wrapper -->
<div class="min-h-screen bg-gray-100">
    <main class="container mx-auto p-6">
        <slot />  <!-- Required to render child pages -->
    </main>
</div>
