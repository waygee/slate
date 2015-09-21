# Errors

<aside class="notice">This error section is stored in a separate file in `includes/_errors.md`. Slate allows you to optionally separate out your docs into many files...just save them to the `includes` folder and add them to the top of your `index.md`'s frontmatter. Files are included in the order listed.</aside>

The HIT API uses the following error codes:


| Status      | Message                                           |
|-------------|---------------------------------------------------|
| 200         | Success                                           |
| 202         | Accepted                                          |
| 203         | Partial information                               |
| 400         | Bad request - malformed request                   |
| 401         | Not authorized                                    |
| 412         | Precondition Failed   |
| 500         | Failure - something went wrong in the code        |

and the following error codes related to data submitted and results returned

| Status      | Message                                           |
|-------------|---------------------------------------------------|
| 1000        | Incorrect input - one or more fields missing      |
| 1001        | No result found                                   |
| 1002        | More than one result found - expecting just one   |
| 1003        | One result found - expecting more than one        |
| 1004        | One result found - expecting no result            |
