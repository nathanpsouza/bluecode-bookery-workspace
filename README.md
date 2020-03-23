# BlueCode API Test

## Dependencies

This projects uses [docker](https://www.docker.com/) and [docker-compose](https://docs.docker.com/compose/), so before run this code, you should have both of then installed on your computer.

## How to run this project

To download this project, just run:

    git clone git@github.com:nathanpsouza/bluecode-bookery-workspace.git &&\
    cd bluecode-bookery-workspace &&\
    git clone git@github.com:nathanpsouza/bookery.git

When the above command is completed, you can setup database with:

    docker-compose run api bash -c 'bundle exec rails db:create db:migrate db:seed'

### Run server

    docker-compose up api

After this, the endpoints will be accessible through `http://localhost:3000`.

### Run Rspec Tests

    docker-compose run api bash -c 'bundle exec rspec spec'

## Headers to use

To send requests, you must provite the `content-type` header with `application/json` value.

## Books endpoints

### Listing all books 

    GET http://localhost:3000/books

This will return a list with all books on api:

    {
      "books": [
        {
          "slug": "vel-voluptatibus-enim-commodi",
          "description": "Eveniet quia et. Velit dolorem ducimus. Dolorem aperiam animi. Perferendis veritatis quia. Magni nihil dolores. Facere culpa sint. Quo ea et. Earum et cum. Sit tenetur reiciendis.",
          "title": "Vel voluptatibus enim commodi."
        },
        {
          "slug": "in-consequatur-natus-repellat",
          "description": "Placeat delectus modi. Similique vero alias. Incidunt magnam et. Quo exercitationem tenetur. Impedit sed ducimus. Magni voluptate officiis. Ut qui rerum. Quia esse veritatis. Quam aperiam aut.",
          "title": "In consequatur natus repellat."
        },
        {
          "slug": "et-quia-repellat-quo",
          "description": "Debitis voluptatem error. Qui ut aut. Vel architecto voluptatum. Iure quos distinctio. Corporis odit porro. Dolor aut architecto. In earum velit. Non ea voluptatem. Nihil mollitia eos.",
          "title": "Et quia repellat quo."
        }
      ]
    }

### Listing one book

Do a GET request to `http://localhost:3000/books/<book_slug>` for example:

    GET http://localhost:3000/books/et-quia-repellat-quo

The response will be a json with details about the book

    {
      "book": {
        "slug": "et-quia-repellat-quo",
        "description": "Debitis voluptatem error. Qui ut aut. Vel architecto voluptatum. Iure quos distinctio. Corporis odit porro. Dolor aut architecto. In earum velit. Non ea voluptatem. Nihil mollitia eos.",
        "title": "Et quia repellat quo."
      }
    }

### Create a new book

Do a post request to `http://localhost:3000/books` with json in body, the body should have the following format:

    {
      "book": {
        "title": "test book.",
        "description": "Cumque explicabo illo. Ut suscipit aut. Quibusdam quam odit. Aut et suscipit. Unde qui consequatur. Nesciunt atque et. Veniam aut explicabo. Eos iusto nisi. Ut iure quia."
      }
    }

the result will be:

    {
      "book": {
        "slug": "test-book",
        "description": "Cumque explicabo illo. Ut suscipit aut. Quibusdam quam odit. Aut et suscipit. Unde qui consequatur. Nesciunt atque et. Veniam aut explicabo. Eos iusto nisi. Ut iure quia.",
        "title": "Test Book"
      } 
    }

### Update existing book

Do a PUT request to `http://localhost:3000/books/<book_slug>` with json in body, for example, to update the test-book:

    PUT http://localhost:3000/books/test-book

the request body should have the following format:

    {
      "book": {
        "description": "new description"
      }
    }

the result will be:

    {
      "book": {
        "slug": "test-book",
        "description": "new description",
        "title": "Test Book"
      } 
    }

### Deleting one book

Do a DELETE request to `http://localhost:3000/books/<book_slug>`, for example:

    DELETE http://localhost:3000/books/test-book

This will return http status ok, without any content.

## Pages endpoints

### Listing all pages 

    GET http://localhost:3000/books/<book-slug>/pages

This will return a list with all books on api:

    {
      "pages": [
        {
          "id": 1,
          "content": "## Itaque\nAt qui enim. Cupiditate dolorum exercitationem. Ut nihil veritatis.\n* Voluptatem. \n* Possimus. \n* Fuga. \n* Qui. \n* Rem. \n",
          "page_number": 1
        },
        {
          "id": 2,
          "content": "### Consequatur\nNon voluptatem animi. Temporibus ad neque. Fugit modi rem.\nAt aspernatur impedit. Et quibusdam quis. ~Dolorum~ eligendi et.",
          "page_number": 2
        },
        {
          "id": 3,
          "content": "##### Ratione\nEum molestiae alias. Esse ea dolorem. Voluptatem ipsa iusto.\n**Quod** totam ut. Ut quia voluptatibus. Maxime quibusdam nisi.",
          "page_number": 3
        }
      ]
    }

### Listing one page

Do a GET request to `http://localhost:3000/books/<book_slug>/pages/<page_number>` for example:

    GET http://localhost:3000/books/et-quia-repellat-quo/pages/1

The response will be a json with details about the page

    {
      "page": {
        "id": 1,
        "content": "## Itaque\nAt qui enim. Cupiditate dolorum exercitationem. Ut nihil veritatis.\n* Voluptatem. \n* Possimus. \n* Fuga. \n* Qui. \n* Rem. \n",
        "page_number": 1
      }
    }

if you ant, you can provide the format for content HTML or TXT, by passing a format param:

    GET http://localhost:3000/books/et-quia-repellat-quo/pages/1?format=html

the content will have the html format

    {
      "page": {
        "id": 1,
        "content": "<h2>Itaque</h2>\n\n<p>At qui enim. Cupiditate dolorum exercitationem. Ut nihil veritatis.\n* Voluptatem. \n* Possimus. \n* Fuga. \n* Qui. \n* Rem. </p>\n",
        "page_number": 1
      }
    }

### Create a new page

Do a post request to `http://localhost:3000/books/<book_slug>/pages` with json in body, the body should have the following format:

    {
      "page": {
        "content": "#### Autem\\nRem quis esse. Quo rerum eos. Fugiat velit dolores.\\n`Natus.`",
        "page_number": 15
      }
    }

the result will be:

    {
      "page": {
        "id": 132,
        "content": "#### Autem\\nRem quis esse. Quo rerum eos. Fugiat velit dolores.\\n`Natus.`",
        "page_number": 15
      }
    }

### Update existing page

Do a PUT request to `http://localhost:3000/books/<book_slug>/pages/<page_number>` with json in body, for example, to update the test-book page number 1:

    PUT http://localhost:3000/books/test-book/pages/1

the request body should have the following format:

    {
      "page": {
        "content": "*new content*"
      }
    }

the result will be:

    {
      "page": {
        "id": 123,
        "page_number": 1,
        "content": "*new content*"
      } 
    }

### Deleting one page

Do a DELETE request to `http://localhost:3000/books/<book_slug>/pages/<page_number>`, for example:

    DELETE http://localhost:3000/books/test-book/pages/1

This will return http status ok, without any content.