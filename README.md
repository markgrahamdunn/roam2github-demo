# roam2github-demo

For those who are having issues with [MatthieuBizien/roam-to-git](https://github.com/MatthieuBizien/roam-to-git/) in GitHub Actions, try my new solution:

1. Add secrets for the following values:

    - `R2G_EMAIL`
    - `R2G_PASSWORD`
    - `R2G_GRAPH`
        - Now with multi graph support! Just add graph names on separate lines in `R2G_GRAPH`. (Or separate them by commas.)
    
2. Update your main.yml with the code here: https://github.com/everruler12/roam2github-demo/blob/main/.github/workflows/main.yml

    - _If you created your repo before October 1st, 2020, you may need to change the branch name from 'main' to 'master'_

This will backup JSON and EDN, but not Markdown (yet)

# **Main README at https://github.com/everruler12/roam2github**

---

### Common error causes

- `R2G ERROR - Secrets error: R2G_EMAIL not found` (or `R2G_PASSWORD` or `R2G_GRAPH`)

    One of those secrets is blank or missing. Add it in Settings > Secrets
    
- `R2G ERROR - Login error. Roam says: "There is no user record corresponding to this identifier. The user may have been deleted."` or `R2G ERROR - Login error. Roam says: "The email address is badly formatted."`

    Your `R2G_EMAIL` secret is incorrect. Try updating it.
    
- `R2G ERROR - Login error. Roam says: "The password is invalid or the user does not have a password."`

    Your `R2G_PASSWORD` secret is incorrect. Try updating it.
    
    Make sure you're not using a Google account login, as this is not supported. (If you are, sign out of Roam, and on the sign-in page, click "Forgot your password" to set a password.)
    
- Timed out with `R2G astrolabe spinning...`. Possible causes I can think of:

    - Roam's servers timed out. Try re-running the job later.

    - Your `R2G_GRAPH` secret is incorrect. Try updating it (make sure it's only the graph name, not a URL)
    
    - You don't have permission to view that graph. (I have been able to get it stuck on the spinning astrolabe when trying to backup someone else's private graph, or using random characters as the graph name in `R2G_GRAPH`)
    
    - Unknown cause. Test with a different graph. Let me know if it happens consistently.
    
    - You graph is too large to be loaded within the backup timeout (default set to 10 minutes). This is highly unlikely, as it shouldn't take 10 minutes to load. (If you still think this is the case, you could try increasing the timeout in main.yml)

- `R2G ERROR - EDN formatting error: mismatch with original`

    The file integrity check to make sure the formatted version of the EDN file matches the downloaded EDN export failed. Please let me know if this were ever to happen.

- `R2G - Clicking "Sign In"` then `R2G ERROR - Error: Protocol error (DOM.describeNode): Cannot find context with specified id`

    I sometimes get this error while testing. I think it's related to Roam loading the login screen a second time, in the middle of trying to sign in (as mentioned in this issue https://github.com/MatthieuBizien/roam-to-git/issues/87#issuecomment-763281895) A check for this and login retry will be added soon...
