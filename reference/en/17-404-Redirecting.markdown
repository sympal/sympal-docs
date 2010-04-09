You can easily redirect a URL that would otherwise cause a 404 error to another URL or a selected content record. Here is an example where we redirect an old chapter of the Sympal documentation to the new chapter:

[asset:104 title=Example redirect]

A redirect contains the following properties:

* **Source** - The source URL that would cause a 404 error to redirect. This can be a pattern of a route to redirect even.
* **Destination** - The URL or route to redirect to.
* **Content** - The content record to redirect to.

## Route Redirecting

Sympal makes it easy to redirect entire routes. For your convenience we have created the `sympal:redirect-route` task. Imagine we had a blog with posts located at `/my_blog/:slug` and we wanted to change it to `/blog/:slug`. Here is the original route:

    [yml]
    old_blog:
      url:   /old_blog/:slug
      param:
        module: blog
        action: view

Now we can simply redirect that route:

    $ php symfony sympal:redirect-route sympal old_blog @new_blog

Now it is safe to remove the `old_blog` route and write our new route:

    [yml]
    new_blog:
      url:   /blog/:slug
      param:
        module: blog
        action: view

Now the old URLs will be redirected to the new blog URL. When we navigate to the blog post at the old URL it will successfully redirect to the new route.