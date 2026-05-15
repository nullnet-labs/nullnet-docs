# Why Aren't Next.js \<Link> and \<Image> Being Used Everywhere?

These components exist to reduce unnecessary loading in Next.js applications, so their exclusion may seem questionable. However, their exclusion is deliberate, and that's because this application includes design decisions that happen to mitigate the optimizations that these components normally provide.

## Link Component Exclusion

The Link component provides pre-fetching of linked page content & preservation of in-browser application state between pages, allowing for smooth client-side navigation between routes. However, THIS application is designed to render lightweight pages on the server before serving them from a performant cache, rather than attempting to build a persistent client-side application state. Due to pages containing dynamic user-generated tags with associated hyperlinks (and additional link sets due to this application documenting websites), large & arbitrary link sets are to be accounted for in the application's design, for which speculative route prefetching provides unnecessary network & runtime overhead compared to using a simple HTML \<a> tag.

To send Link components, even without pre-fetched page information due to setting "prefetch" to false, is to add overhead per hyperlink, overhead that isn't being used. Since page transitions don't require preserving client UI state & pre-fetching may in fact be detrimental in this context, this application maintains a preference for using \<a> tags instead of \<Link> components for its pagination requirements.

## Image Component Exclusion

The Image component provides an automatic image optimization pipeline for rendering images to Web pages across devices. However, this application sidesteps the need for such optimizations, by providing pre-rendered images that are statically sized across browsing environments. By leveraging this, the \<img loading="lazy"> component becomes sufficient to fulfill image display needs, without the overhead that comes with defaulting to \<Image> components.
