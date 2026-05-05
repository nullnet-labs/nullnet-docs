# Human Curation

What is meant by "human-curated" in this application?

## Summary

A human-curated system is a content organization model where the content's discovery, relationships, and categorizations are largely driven by human judgment rather than algorithmic optimization.

## Curation in Context

In this project, human curation means:

- Users can assume fine control over which content they can discover
- Content categorization & related content is delivered based on human-defined meaning, not just engagement metrics
- Popularity ranking, if implemented, is just another optional discovery metric that a user can choose to engage with or not, rather than being an entire gatekeeping mechanism for which content is & is not delivered to users

## Existing Systems

This concept is informed by several existing systems that demonstrate various interpretations of human-curated content organization:

- [Wikipedia](https://www.wikipedia.org/)
  - content governed directly by community
  - discovery via structure, through links between pieces of content
- [Newgrounds](https://www.newgrounds.com/)
  - passes content through a quality control "portal" system where the community uses a rating system to choose what stays and what goes
  - main discoverability method (the front page) is largely based on an intersection between popularity, recency, and light editorial control
  - alternative discoverability methods include content-type-specific pages, content creator pages, a user forum, archives of past popular content, all-time rankings, a simple tagging system, a search-by-name system, and user-curated lists as "related content" sections
- [Steam Curators](https://store.steampowered.com/curators/topcurators/)
  - individual users assemble and rate gaming & software content on Valve's Steam platform
  - this is a discoverability-focused sub-system of Steam, but on the page dedicated to curators, the discoverability of curators themselves is popularity-based
  - curators may additionally be found in a small "What Curators Say" section on the Steam store pages for content postings
- [Neocities](https://neocities.org/browse)
  - minimally curated website list with selectable metrics, simple pagination to find further content, a simple tagging system, and user pages showing who follows whom
  - an equally if not more important discovery practice is classic-Web link surfing & webrings between the sites themselves, all custom set by the Neocities users who run the pages
- [*booru sites](https://safebooru.org/)
  - massively tag-driven art sharing platforms with user-defined tags, tags assigned to content postings by the user community, and minimal algorithmic interference
  - includes understated discovery features like "content pools" (user curations of content) and tag aliases to capture searches for similar tags (get "amber_eyes" results when someone searches "golden_eyes")
  - tags include depth-adding features such as a user-run dictionary for defining tags, and tag types (like content-descriptive tags, metadata tags, tags for who originally created a piece of content, and tags for which copyrighted work is being referenced by fanart)
- [Rate Your Music](https://rateyourmusic.com/)
  - front-page experience encourages finding content through editorially selected user reviews & recently popular content additions
  - deeply user-curated charts, lists, and genre taxonomies
- [Bandcamp](https://bandcamp.com/)
  - front-page experience includes a real-time chronological feed of content that users have VERY recently engaged with, followed by news-site-style editorial highlights of recent content additions & uploads that have been relatively popular over the past day
  - dedicated "discover" pages with tag-driven genre/sub-genre selector, leveraging artist-defined tagging and, if the artist chooses, geographically located music scenes
  - community features that further boost discovery, including "Bandcamp Friday" sharing events, artist following, and multi-artist aggregations by music labels
- Web forums, chat applications, and Futaba/chan-style imageboards
  - theoretically 100% human-driven content
  - content discovery via discussion & chronology, not via personalized feeds

### Recurring patterns
- Contextual meaning-based grouping
  - galleries / lists
  - ESPECIALLY tags
- Relatively low or even absent algorithmic content personalization
- Content recency as a default discoverability metric
- Editorial content highlights on top of or in addition to community-curated content
- Search is common, though it's of varying quality

### Extraordinary patterns
- Extensive & highly fleshed-out tagging support (*booru sites)
- Content views that allow old content to be "bumped" back into a high-visibility location (forums & imageboards, as well as Neocities "Recently Updated" site browsing)
- Popular-within-multiple-timeframes non-personalized content feeds (Bandcamp, for displaying content being engaged with at the moment, within the day, or within the last several days)

## Project Design Implications

To fulfill human curation as a first-class design input, this project will include:

- Tagging support
  - User-defined tags
  - Tag assignment to content that's handled by the user community, not just the content poster
  - Multi-tag searching
- Visibility controls that can fully exclude engagement metrics
- No single optimal ranking system
- User-created collections

## Boundaries

This project will NOT include:

- Non-user-defined personalization of content visibility
- Purely editorially / administratively restrictive posting permissions (like traditional publishing)
- Ranking & discovery systems based on hidden machine-defined metrics & algorithms

## Open Questions

- How will user attention be focused onto the baseline curation features?
- What curation features can be added that are unique to featuring Web pages?
- Can content quality be maintained through tagging, or should there be a categorical measure that allows low-quality content to be hidden by default?
