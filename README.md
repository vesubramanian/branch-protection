
# Set / Delete Branch Protection


This action will set / delete Branch Protection rules on specified branches of GitHub Repositories within an organization. Same rules will get applied on the specified branches for all the GitHub Repositories of an organization. The Rules JSON (format provided below in the inputs) should have the branches and rules as kev value pairs. The below JSON sample specifies the rules for Main and Release branches. The token used below should be of an Admin user.

**Note**: Use this action after the Checkout step in your workflow, since we are specifying the paths of rules JSON and other files and we have to make sure the files are available for use by this action. Hence it is recommended to use this action after the Checkout step.

# Inputs

### `token`
**Required**  
**Description** - This should be the token of a GitHub Admin / Organization Owner to be able to manage all repos within the organization, passed as a secret.

### `org`
**Required**  
**Description** - Name of your GitHub Organization.  

### `rulesPath`
**Required**  
**Description** - The path of the Rules JSON file with Branch name as Key and Rules as Value. Format of the file is given below. The below JSON applies branch protection rules to release and main branches of a repo.

{ 
    "release" : {
    "restrictions": {
                        "users": [""],
                        "teams": [""]
                    },                              
    "required_status_checks": null,
    "enforce_admins": true,
    "required_linear_history": true,
    "required_pull_request_reviews": {
        "dismiss_stale_reviews": true
    }
  },
  "main": {
    "restrictions":null,                              
    "required_status_checks": null,
    "enforce_admins": null,
    "required_linear_history": null,
    "required_pull_request_reviews": {
    "dismiss_stale_reviews": true
    }
  }
}

### `includedReposPath`
**Description** - Path of the text file (.txt) with only those repos on which the branch protection needs to be applied. Put every repo name (without org/ prefix) on a new line. This is optional. If not provided, branch protection will be set on all the repos within the organization.

### `excludedReposPath`
**Description** - Path of the text file(.txt) with repos to be excluded from branch protection. Put every repo name (without org/ prefix) on a new line. This is optional. If not provided, no repo will be excluded and branch protection will be applied on all the repos within the organization.

### `action`
**Description** - This GitHub Custom action can be used to set / delete branch protection. The default value is set (if not specified). If delete is assigned, it will remove branch protection from every repo, if already present.
**Default** - 'set'  


# Usage

```

- name: Run Branch Protection
  uses: venh/branch-protection@main
  with:
    token: '${{ secrets.GITHUB_ADMIN_ACCESS_TOKEN }}' 
    org: Name-Of-Your-GitHub-Organization 
    rulesPath: ./rules.json 
    includedReposPath: ./includedRepos.txt
    excludedReposPath: ./excludedRepos.txt
    action: set
```
# License

This project is released under the [MIT License](LICENSE)
