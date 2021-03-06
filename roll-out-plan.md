# SAML Plan Rollout

## Week 0

- What is SAML?
  - https://github.com/jch/saml/blob/master/README.md
- What is SSO?
  - [IAM with SAML SSO](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/about-identity-and-access-management-with-saml-single-sign-on)
  - [Official Supported SAML services](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/about-identity-and-access-management-with-saml-single-sign-on#supported-saml-services)

## Week 1

- [ ] GitHub purchased + upgraded
- [ ] Configure testing
  - Cloud: For GHEC you can [create a new org](https://docs.github.com/en/github/setting-up-and-managing-your-enterprise-account/adding-organizations-to-your-enterprise-account) within your Enterprise to use for testing or use a current org but do not switch on the “enforce” option. Once successfully configured users will see an “authenticate with saml” banner so using a separate org might be needed to avoid users seeing this.
  - Server: For GHES we recommend using a [test/staging instance of GHES](https://docs.github.com/en/enterprise/2.22/admin/installation/setting-up-a-staging-instance) as there is no enable but not enforce option so once enabled all users will need to authenticate through SAML.
- [ ] Begin manager communication, ideally at [Engineering Managers Meeting](./communication-templates/week-2-manager.md)
- [ ] Create documentation plan for tracking information about users, admins, and bot accounts
  - [Managing bots and service accounts with SAML SSO](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/managing-bots-and-service-accounts-with-saml-single-sign-on)

## Week 2

### Audit Users

- [ ] Audit User activity in repo
  - [GHEC Script: Find inactive users](https://github.com/github/platform-samples/tree/master/api/ruby/find-inactive-members)
- [ ] Audit Admins who don’t need to be admins
  - Manually look through admin permissions. Make note of anything that goes against expectations. Follow up and communicate to see why this is necessary.
  - [Script: GHE org permissions report](https://github.com/github/platform-samples/blob/master/api/ruby/ghe-org-permissions-report.rb)
- [ ] Audit Bots to be marked as outside collaborators
  - This is done primarily through communication
  - On GHEC, you can see outside collaborators through the outside_collaborators tab: `https://github.com/enterprises/<enterprise>/outside_collaborators`
- [ ] Choose a plan for bots/service accounts.
  - [Help: Managing bots and service accounts with SAML single sign on](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/managing-bots-and-service-accounts-with-saml-single-sign-on)
  - Options include:
    - [Adding service accounts as outside collaborators](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/adding-outside-collaborators-to-repositories-in-your-organization)
    - Create a separate organization that isn't SAML enforced (GHEC only)
      - Repositories can be forked into this organization and marked as `Internal`.
    - [Allow built in authentication for users outside of your identity provider](https://docs.github.com/en/enterprise/2.22/admin/authentication/allowing-built-in-authentication-for-users-outside-your-identity-provider) (GHES only)

### Testing & Initial Configuration

- [ ] Configure initial testing environment with identity provider
  - [Enabling and testing SAML single sign on for your organization](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/enabling-and-testing-saml-single-sign-on-for-your-organization)
  - **GHES**: 
    - [SAML attritubtes](https://docs.github.com/en/enterprise/2.22/admin/authentication/using-saml#saml-attributes)
    - If you are migrating from LDAP to SAML and would like to continue using Team Sync with SAML, we have this open-source utility:[GHEC: Script for SAML and Team Sync - active directory](https://github.com/github/saml-ldap-team-sync)
  - [GHEC Mapping SAML to GitHub IDs: Script to pull details to help cross reference](https://github.com/github/platform-samples/blob/master/graphql/queries/enterprise-sso-member-details.graphql)
- [ ] Decide on 2FA plan
  - If you choose to configure 2FA on GitHub, here are [instructions](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/keeping-your-organization-secure)

### Communication

- [ ] Begin [user communication](./communication-templates/wee-k3-admin-to-users.md)
- [ ] Message inactive Admins to ensure that they do not need access before downgrading
- [ ] Identify pool of test users and bots based on manager and engineer feedback

## Week 3

- [ ] Enact plan on managing bots and services accounts
- [ ] Remove inactive admins
- [ ] Remove inactive bots
- [ ] Remove inactive users
- [ ] Enable SAML
  - [GHEC: Enabling and testing SAML single sign on for your organization](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/enabling-and-testing-saml-single-sign-on-for-your-organization)
  - [GHES: Configuring SAML](https://docs.github.com/en/enterprise/2.22/admin/authentication/using-saml#configuring-saml-settings)
  - [GHEC: Verifying your organization's domain](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/verifying-your-organizations-domain)
  - [GHEC: How to restrict notifications to your domain only](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/restricting-email-notifications-to-an-approved-domain)
  - [Azure AD, Okta, and OneLogin: About SCIM](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/about-scim)
- [ ] Add GitHub to IdP
- [ ] Manually provision GitHub to test users
- [ ] Communicate to test users
- [ ] Manual testing by users in test pool
- [ ] Ensure bots can deploy when marked as outside collaborators

## Week 4

- [ ] Provision GitHub to all of Engineering via IdP
  - [Preparing to enforce SAML single sign on](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/preparing-to-enforce-saml-single-sign-on-in-your-organization)
- [ ] Add GitHub to appropriate IdP group
- [ ] Announcement email
  - GHEC Users will need to:
    - [Authorize personal access tokens for use with SAML](https://docs.github.com/en/github/authenticating-to-github/authorizing-a-personal-access-token-for-use-with-saml-single-sign-on)
    - [Authorize SSH keys for use with SAML](https://docs.github.com/en/github/authenticating-to-github/authorizing-an-ssh-key-for-use-with-saml-single-sign-on)

## Week 5

- [ ] Audit for adoption
  - [GHEC: script to run via GraphQL](https://github.com/github/platform-samples/blob/master/graphql/queries/users-in-org-with-sso.graphql)
  - Server: As you click the button, you will be prompted with list of users who do not yet have it enabled, and asked if you are sure you want to enforce
- [ ] Resolve unverified accounts
- [ ] Prepare response team for SAML enforcement

## Week 6

- [ ] Wrap up communication
- [ ] Rollout / SSO Required in GitHub
  - Users not authed via Okta will automatically removed from of the Org and will need to auth via Okta and be manually added to the Dev Team. You can find [instructions here](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/reinstating-a-former-member-of-your-organization).
  - [Enforcing SAML](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/enforcing-saml-single-sign-on-for-your-organization)
- [ ] Response team available for immediate auth needs

## Beyond

In the future, when a user is provisioned GitHub in the SAML IdP, they will receive an GitHub Organization invite email from GitHub. The user should click on the link in the email and complete the SSO flow. This will be the standard for all new users once SAML authentication is enforced.
