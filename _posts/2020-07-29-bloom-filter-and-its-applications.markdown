---
layout: post
title:  "Bloom Filter and Its use in Git"
date:   2020-07-29 20:28:52 +0400
categories: data_structures git
---
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight c %}
struct commit;
struct repository;

struct bloom_filter_settings {
\
    uint32_t hash_version;

    /*
     * The number of times a path is hashed, i.e. the
     * number of bit positions tht cumulatively
     * determine whether a path is present in the
     * Bloom filter.
     */
    uint32_t num_hashes;

    /*
     * The minimum number of bits per entry in the Bloom
     * filter. If the filter contains 'n' entries, then
     * filter size is the minimum number of 8-bit words
     * that contain n*b bits.
     */
    uint32_t bits_per_entry;
};

#define DEFAULT_BLOOM_FILTER_SETTINGS { 1, 7, 10 }
#define BITS_PER_WORD 8
#define BLOOMDATA_CHUNK_HEADER_SIZE 3 * sizeof(uint32_t)

struct bloom_filter {
    unsigned char *data;
    size_t len;
};

struct bloom_key {
    uint32_t *hashes;
};

uint32_t murmur3_seeded(uint32_t seed, const char *data, size_t len);

void fill_bloom_key(const char *data,
            size_t len,
            struct bloom_key *key,
            const struct bloom_filter_settings *settings);
void clear_bloom_key(struct bloom_key *key);

void add_key_to_filter(const struct bloom_key *key,
               struct bloom_filter *filter,
               const struct bloom_filter_settings *settings);

void init_bloom_filters(void);

struct bloom_filter *get_bloom_filter(struct repository *r,
                      struct commit *c,
                      int compute_if_not_present);

int bloom_filter_contains(const struct bloom_filter *filter,
              const struct bloom_key *key,
              const struct bloom_filter_settings *settings);

{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
