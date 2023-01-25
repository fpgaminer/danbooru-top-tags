# danbooru-top-tags

Scripts and notebooks for build a "top tag" list from Danbooru datasets. This tag list is useful for training neural networks that, for example, predict tags for images.

Main file is `BuildTopTags.ipynb`.

Note that currently this code depends on my custom Danbooru database.

Methodology:

* Go through all Danbooru metadata
* Skip posts with images that cannot be decoded into PIL.Image
   * This excludes gifs, webms, etc. as well as broken images.
   * These images cannot be trained on in other parts of the pipeline, so their tags are not useful.
* Skip tags listed in tag_blacklist.txt
   * This is a hand built list of tags that I feel aren't useful.
   * For example, tags like "bad_id" are not relavant to the image itself.
   * It's possible to generate a blacklist in an automated way from things like the metatags category (https://danbooru.donmai.us/wiki_pages/tag_group%3Ametatags), but I think manually building the blacklist, though tedious, would be more accurate.
   * I did not blacklist Tag Group: Text tags, since I believe other networks in the pipeline will be able to handle them or infer some useful information from them.
* Tag aliases are resolved to their canonical tag.
* Tag implications are applied.
   * This should help get a better tag usage count for higher order tags.
* After all that filtering, tag usage counts are calculated.
* Tags are sorted by usage count.
* The top 6000 tags are selected.
   * This is based on the first round number where all tags are represented at least 1000 times.
* The tag list is saved to `top_tags.txt`.