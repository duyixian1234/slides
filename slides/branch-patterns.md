---
marp: true
theme: gaia
paginate: true
---

# Branch Patterns

duyixian 2021-05-03

---

## Background

Martin Fowler
[Patterns for Managing Source Code Branches](https://martinfowler.com/articles/branching-patterns.html)

> Branching is easy, Merging is harder.

---

### Main Points

Branches should be integrated frequently and efforts focused on a healthy mainline that can be deployed into production with minimal effort.

---

## Base Patterns

- Integration
- Production

---

### Source Branching

Create a copy and record all changes to that copy.

---

Why Branching?

> If several people work on the same code base, it quickly becomes impossible for them to work on the same files.

- Each developer takes a copy of the code base called branch.
- Record every change made to each branch as commit.

---

A branch is a particular sequence of commits to the code base.

![branch](https://martinfowler.com/articles/branching-patterns/series-commits.png)

---

Branches merge when commits from one branch are applied to another.

![merge](https://martinfowler.com/articles/branching-patterns/split-and-merge.png)

---

In distributed version control systems(git),different forks's master branches are separate branches.

![different master](https://martinfowler.com/articles/branching-patterns/branch-and-tag.png)

---

### Codeline

> A codeline is a particular sequence of versions of the code base. It can end in a tag, be a branch, or be lost in git's reflog.

---

### Conflicts

#### Textual Conflicts

Origin

```javascript
const fn(){
    console.log('Something.')
}
```

---

Tom's commit

```javascript
const fn(){
    console.log('Something Else.')
}
```

Mike's commit

```javascript
const fn(){
    console.log('Nothing.')
}
```

---

#### Sematic Conflicts

Tom's commit

```javascript
const ffn(){
    console.log('Something.')
}
```

Mike's commit

```javascript
// other.js
fn();
```

---

Actual Branching Diagrams
![branching diagrams](https://martinfowler.com/articles/branching-patterns/leroy-branch.jpg)

---

### Mainline

A single, shared, branch that acts as the current state of the product

git: upstream/master

---

> keep up to date with changes in the mainline by pulling mainline's changes at intervals and merging them into my personal development branch.

```bash
checkout work-branch
# do some work
git add . & git commit -m "some message"
git pull upstream master [--rebase]
git push
```

---

### Healthy Branch

On each commit, perform automated checks, usually building and running tests, to ensure there are no defects on the branch

- Commit Suite(Pre-Commit Hooks)
- Static Analysis(lint, format)
- Unit Tests
- Self Testing Code
- Deployment Pipeline
- Pre-Integration Review

---

## Integration Patterns

> Branching is about managing the interplay of isolation and integration.

---

### Mainline Integration

Developers integrate their work by pulling from mainline, merging, and - if healthy - pushing back into mainline

---

checkout and making commits
![mainline-integration-checkout](https://martinfowler.com/articles/branching-patterns/mainline-integration-checkout.png)

mainline accept other updates
![mainline-integration-other-update](https://martinfowler.com/articles/branching-patterns/mainline-integration-other-update.png)

---

pull from mainline
![mainline-integration-pull](https://martinfowler.com/articles/branching-patterns/mainline-integration-pull.png)

merge from mainline
![mainline-integration-fuse](https://martinfowler.com/articles/branching-patterns/mainline-integration-fuse.png)

---

push (and merged )to mainline
![mainline-integration-integrate](https://martinfowler.com/articles/branching-patterns/mainline-integration-integrate.png)

---

### Feature Branching

Put all work for a feature on its own branch, integrate into mainline when the feature is complete.

---

working on feature branch
![fb-start](https://martinfowler.com/articles/branching-patterns/fb-start.png)

pull from mainline
![fb-pull](https://martinfowler.com/articles/branching-patterns/fb-pull.png)

---

integrate to mainline
![fb-integrate](https://martinfowler.com/articles/branching-patterns/fb-integrate.png)

---

### Integration Frequency

---

#### Low-Frequency Integration

![low-freq-start](https://martinfowler.com/articles/branching-patterns/low-freq-start.png)

![low-freq-M1](https://martinfowler.com/articles/branching-patterns/low-freq-M1.png)

---

![low-freq-SM](https://martinfowler.com/articles/branching-patterns/low-freq-SM.png)

![low-freq-VM](https://martinfowler.com/articles/branching-patterns/low-freq-VM.png)

---

![low-freq-S-push](https://martinfowler.com/articles/branching-patterns/low-freq-S-push.png)

![low-freq-V-push](https://martinfowler.com/articles/branching-patterns/low-freq-V-push.png)

---

#### High-Frequency Integration

![high-freq-V1](https://martinfowler.com/articles/branching-patterns/high-freq-V1.png)

---

![high-freq-S1](https://martinfowler.com/articles/branching-patterns/high-freq-S1.png)

![high-freq-V2S2](https://martinfowler.com/articles/branching-patterns/high-freq-V2S2.png)

---

![high-freq-M1S3](https://martinfowler.com/articles/branching-patterns/high-freq-M1S3.png)

![https://martinfowler.com/articles/branching-patterns/high-freq-V6.png](https://martinfowler.com/articles/branching-patterns/high-freq-M1S3.png)

---

Low Frequency

![low-freq-conflict](https://martinfowler.com/articles/branching-patterns/low-freq-conflict.png)

---

High Frequency

![high-freq-conflict.](https://martinfowler.com/articles/branching-patterns/high-freq-conflict.png)

---

### Continuous Integration

Developers do mainline integration as soon as they have a healthy commit they can share, usually less than a day's work

> "everyone commits to the mainline every day"

---

Frequency Reduces Difficulty

![fb-vs-ci](https://i.loli.net/2021/05/02/LxD5srCbgNJvhPz.png)

---

### Pre-Integration Review

Every commit to mainline is peer-reviewed before the commit is accepted.

Pull Request

---

### Integration Friction

Manual process

---

### Modularity and Integration

> Feature Branching is a poor man's modular architecture, instead of building systems with the ability to easy swap in and out features at runtime/deploytime they couple themselves to the source control providing this mechanism through manual merging.
> -- Dan Bodart

---

## The path from mainline to production release

Continuous Delivery

![mainline-release](https://martinfowler.com/articles/branching-patterns/mainline-release.png)

---

### Release Branch

A branch that only accepts commits accepted to stabilize a version of the product ready for release.

![apply-to-release](https://martinfowler.com/articles/branching-patterns/apply-to-release.png)

---

![apply-to-mainline](https://martinfowler.com/articles/branching-patterns/apply-to-mainline.png)

---

![multi-release](https://martinfowler.com/articles/branching-patterns/multi-release.png)

---

### Maturity Branch

production branch

![production-branch](https://martinfowler.com/articles/branching-patterns/production-branch.png)

---

Long Lived Release Branch

![long-running-release](https://martinfowler.com/articles/branching-patterns/long-running-release.png)

---

### Environment Branch

Configure a product to run in a new environment by applying a source code commit.

Anti Pattern

---

### Hotfix Branch

A branch to capture work to fix an urgent production defect.

![hotfix-branch](https://martinfowler.com/articles/branching-patterns/hotfix-branch.png)

---

Hotfix on release branch
![hotfix-rb](https://martinfowler.com/articles/branching-patterns/hotfix-rb.png)

Hotfix on mainline
![hotfix-on-mainline](https://martinfowler.com/articles/branching-patterns/hotfix-on-mainline.png)

---

### Release Train

Release on a set interval of time, like trains departing on a regular schedule. Developers choose which train to catch when they have completed their feature.

![release-train-multi](https://martinfowler.com/articles/branching-patterns/release-train-multi.png)

---

### Release-Ready Mainline

Keep mainline sufficiently healthy that the head of mainline can always be put directly into production

![mainline-releas](https://martinfowler.com/articles/branching-patterns/mainline-release.png)

---

## Other Branching Patterns

---

### Experimental Branch

Collects together experimental work on a code base, that's not expected to be merged directly into the product.

---

### Future Branch

A single branch used for changes that are too invasive to be handled with other approaches.

---

### Collaboration Branch

A branch created for a developer to share work with other members of the team without formal integration.

---

### Team Integration Branch

Allow a sub-team to integrate with each other, before integrating with mainline.

---

### Git-flow

Vincent Driessen [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)

- Mainline
- Feature Branch
- Release Branch
- Hotfix Branch

---

### Github Flow

Scott Chacon [Github Flow](http://scottchacon.com/2011/08/31/github-flow.html)

Release-Ready Mainline

- Anything in the _master_ branch is deployable

- To work on something new, create a descriptively named branch off of _master_ (ie: _new-oauth2-scopes_)

- Commit to that branch locally and regularly push your work to the same named branch on the server

---

- When you need feedback or help, or you think the branch is ready for merging, open a pull request

- After someone else has reviewed and signed off on the feature, you can merge it into master

- Once it is merged and pushed to ‘master’, you can and should deploy immediately

---

### Trunk-Based Development

[Introduction](https://trunkbaseddevelopment.com/)

---

### Gitlab Flow

[Introduction](https://docs.gitlab.com/ee/topics/gitlab_flow.html)
[Chinese Version](https://www.jianshu.com/p/0080df2c2d8c)

---

## Thoughts of Branching

> branching is easy, merging is harder. Branching is a powerful technique, but it makes me think of goto statements, global variables, and locks for concurrency. Powerful, easy to use, but easier to over-use, too often they become traps for the unwary and inexperienced.

---

Thanks for watching
