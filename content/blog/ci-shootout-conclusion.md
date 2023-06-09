---
author: Daniel Jones
date: "2022-01-19"
heroImage: /img/blog/ci-shootout/goodbadugly.jpg
title: "CI Shootout: Conclusion"

heading: Our
headingBold: blog
Description: Get the very latest updates about recent projects, team updates, thoughts and industry news from our team of EngineerBetter experts.
---

<figure>
    <figcaption style="font-size: .75em; font-style: italic;">
    The Good, The Bad, The Ugly by <a href="https://www.flickr.com/photos/kaptainkobold/1021866852">Alan</a>
    </figcaption>
</figure>

When should you use Jenkins, Concourse, Tekton or Argo Workflows? What work is each suited to, and what limitations do they each have? How will your choice of CI server affect the way that you work?

After comparing them on a variety of fronts, in this article we comment on how these systems differ and which you should use.

## Recap

You may wish to revisit some of the earlier posts to see the details about how each CI system compares:

* [_First post_](/blog/ci-shootout-getting-started) - 1. **Install** and configure the CI system
* [_First post_](/blog/ci-shootout-getting-started) - 2. **Run** a "Hello, World" task
* [_Second post_](/blog/ci-shootout-inputs-and-outputs) - 3. **Trigger** pipeline runs when external resources (eg Git repos, S3 buckets) change
* [_Second post_](/blog/ci-shootout-inputs-and-outputs) - 4. Use **inputs and outputs to tasks**
* [_Second post_](/blog/ci-shootout-inputs-and-outputs) - 5. Write **outputs externally** (like making a Git commit or pushing a file to S3)
* [_Third post_](/blog/ci-shootout-reuse) - 6. **Re-use** pipeline configuration
* [_Third post_](/blog/ci-shootout-reuse) - 7. Use the CI system as a **build monitor**
* _This post_ - 8. Conclusion

## The scores

It's a really blunt and inaccurate way of evaluating something as complicated as a CI system, but who doesn't love a good comparison table?

Below is a compilation of how each system was rated in the previous articles. We've tallied the scores on a scale of 1&rarr;4 corresponding to Poor&rarr;Great.

<table class="comparison">
  <tr>
    <td>
    </td>
    <th>
      Jenkins
    </th>
    <th>
      Concourse
    </th>
    <th>
      Tekton
    </th>
    <th>
      Argo Workflows
    </th>
  </tr>
  <tr>
    <td>
      Installation
    </td>
    <td>
      Mediocre
    </td>
    <td>
      Great
    </td>
    <td>
      Good
    </td>
    <td>
      Great
    </td>
  </tr>
  <tr>
    <td>
      Running a task
    </td>
    <td>
      Poor
    </td>
    <td>
      Great
    </td>
    <td>
      Great
    </td>
    <td>
      Great
    </td>
  </tr>
  <tr>
    <td>
      Triggering
    </td>
    <td>
      Good
    </td>
    <td>
      Great
    </td>
    <td>
      Poor
    </td>
    <td>
      Mediocre
    </td>
  </tr>
  <tr>
    <td>
      Using inputs
    </td>
    <td>
      Good
    </td>
    <td>
      Great
    </td>
    <td>
      Great
    </td>
    <td>
      Good
    </td>
  </tr>
  <tr>
    <td>
      Write outputs
    </td>
    <td>
      Poor
    </td>
    <td>
      Great
    </td>
    <td>
      Mediocre
    </td>
    <td>
      Mediocre
    </td>
  </tr>
  <tr>
    <td>
      Re-use
    </td>
    <td>
      Good
    </td>
    <td>
      Good
    </td>
    <td>
      Great
    </td>
    <td>
      Mediocre
    </td>
  </tr>
  <tr>
    <td>
      Build monitor
    </td>
    <td>
      Good
    </td>
    <td>
      Great
    </td>
    <td>
      Great
    </td>
    <td>
      Great
    </td>
  </tr>
  <tr>
    <td>
      Overall
    </td>
    <td>
      19
    </td>
    <td>
      31
    </td>
    <td>
      26
    </td>
    <td>
      25
    </td>
  </tr>
</table>

Concourse comes out on top, with Jenkins very much in last place:

<table class="comparison">
  <tr>
    <th>CI System</th>
    <th>Score</th>
    <th>Comments</th>
  </tr>
  <tr>
    <td>Concourse</td>
    <td>31</td>
    <td style="text-align: left;">Best for collaborative continuous delivery teams</td>
  </tr>
  <tr>
    <td>Tekton</td>
    <td>26</td>
    <td style="text-align: left;">Kube-native, needs hooks, verbose, Tekton Hub is great</td>
  </tr>
  <tr>
    <td>Argo Workflows</td>
    <td>25</td>
    <td style="text-align: left;">Kube-native, needs hooks, higher-level than Tekton</td>
  </tr>
  <tr>
    <td>Jenkins</td>
    <td>19</td>
    <td style="text-align: left;">Dated, clunky, flexible, ubiquitous</td>
  </tr>
</table>

Let's discuss each in more detail.

## Jenkins

Jenkins is old and dated. It lacks isolation (workloads aren't containerised by default, there's a shared workspace, and global JVM settings), and the plugin system almost guarantees surprise incompatibilities. That said, Jenkins does offer LTS releases and its use in enterprises increases the likelihood of security fixes being made.

Jenkins allows users to make snowflake servers and builds. Yes, you can get it to persist config to source control, but it allows a workflow that is considered an anti-pattern in a cloud native, GitOps world. It is, however, the only system reviewed that offers CI-as-_code_, and not CI-as-YAML.

Some people view Jenkins not as a CI server, but as a _platform_. This shows in its ability to do pretty much anything. This is as much of a weakness as a strength, as that flexibility allows all sorts of undesirable practices to creep in.

It is job-oriented by default, although newer plugins provide a pipeline view. Much like Tekton and Argo Workflows, Jenkins does not offer a 'single pane of glass' view on the current state of a pipeline.

It is difficult to recommend any new installs of Jenkins. Most competitors are better in every dimension. The one major strength that Jenkins has is that _everyone_ who has been in the industry for more than a couple of years will have used it, and so it isn't hard or expensive to find Jenkins skills.

> ### Use Jenkins when you:
>
> * prefer a manual, GUI-driven workflow
> * need something ubiquitous for which the skills are common
>
> ### Don't use Jenkins if you:
>
> * need to enforce a CI-as-code workflow
> * have a low tolerance for ephemeral issues (dirty workspace, broken plugins)

## Tekton and Argo Workflows commonalities

Both systems have a lot in common, so let's discuss the design choices that they share before considering them in isolation. They really are very similar, and you'll be able to achieve similar outcomes with either.

### Kubernetes-only

Tekton and Argo Workflows are both Kubernetes-native, which means they get to leverage a great container scheduler by default, and are built using modern paradigms.

Container creation on Tekton and Argo Workflows _just works_, which is exactly what you'd want and expect. Given that their data are stored in the Kubernetes API server, there's no additional database to worry about, and all the tooling that Kubernauts are familiar with will be usable here.

There is a question of build history and Kubernetes' underlying etcd storage: sooner or later you'll need to archive build logs off to external storage, because etcd wasn't designed to store large volumes of text.

There's one other problem with being Kubernetes-based - what happens if you don't have a Kubernetes cluster? What if you're writing a pipeline to _create_ a Kubes cluster? Some of us have genuinely been in a conversation with platform engineers who never knew a world before Kubernetes, and who are mystified how to do CI without a Kubernetes cluster.

### Webhooks, public internet access

Neither Tekton nor Argo Workflows make it easy to poll external _things_ (eg Git repos, S3 buckets, FTP servers...), and instead the Kubernetes cluster running these systems need to be addressable from where-ever your code lives. If you're hosting code on public GitHub, then that means your cluster needs to be reachable from the outside world.

This isn't really feasible in an air-gapped, regulated environment like a bank.

### Single entry point

Tekton and Argo Workflows pipelines can only be triggered for one reason - a single code repository changing. If you have lots of reasons for a build to happen (perhaps you're integrating many upstream releases) then you're going to have to do a lot of work to stitch many jobs and pipelines together.

## Tekton specifics

Tekton gives the impression of a set of building blocks from which more productive CI/CD solutions can be made. There's flexibility to be had here, but the low level of abstraction means there are lots of pieces to put together.

### Tekton Hub

Tekton hub is a great idea. As anyone who's spent a few years in the industry will tell you, most CI pipelines end up repeating a lot of simple but error-prone tasks. Being able to leverage those from the community is great, and should be a productivity boon.

> ### Use Tekton if you:
>
> * are building a higher level of abstraction
> * have bespoke needs, and want to craft your own CI/CD system
> * use Slack alerting rather than a build monitor
> * want many small pipelines
>
> ### Don't use Tekton when you:
>
> * cannot use Kubernetes
> * cannot use webhooks
> * need to trigger builds for multiple reasons
> * want a simple domain model
> * want a single view that shows the current state of a project

## Argo Workflows specifics

The suite of Argo components combine to provide a higher level of abstraction than Tekton. Other than that, there's not a lot to separate them.

> ### Use Argo Workflows if you:
>
> * want to get started quickly
> * use Slack alerting rather than a build monitor
> * want many small pipelines
>
> ### Don't use Argo Workflows when you:
>
> * cannot use Kubernetes
> * cannot use webhooks
> * need to trigger builds for multiple reasons
> * want a single view that shows the current state of a project

## Concourse

### Pull request workflows

Concourse has gotten better at visualising pull request-based workflows, but it's still not ideal. It was built with the opinions of its authors baked in, and they practised [true continuous delivery (of which trunk-based development is a core tenet)](https://trunkbaseddevelopment.com/continuous-delivery/). [Instanced pipelines](https://concourse-ci.org/instanced-pipelines.html) make this sort of workflow easier to track, but we still haven't gotten to the [v10 vision](https://blog.concourse-ci.org/core-roadmap-towards-v10/).

### Single pane of glass

Concourse's user interface always shows you the latest version of a pipeline. It's perfectly suited to use as a build monitor, constantly displaying the current state of the world to a team.

This is of more value to teams that practice continuous delivery, and not branch-based development. It wasn't covered in these posts but Concourse's UI has not been great at pull request workflows, although there are plans to change that.

How much use is this in a world of developers working remotely, in isolation, on pull requests? Build monitors may be going out of fashion, but a single snapshot view is handy when debugging.

### Resources, polling, event-driven CI

Concourse resources encapsulate and make reusable polling logic that will reach out and detect changes in various things, like Git repos, S3 buckets, container image registries, Maven repositories, and more. Because Concourse reaches out to the world, you do not need the outside world to be able to reach in to trigger builds.

### Multiple entry points

Concourse was built for _integration_. That means bringing lots of things from lots of places together, to run some tests, see if they work, and then bundle something up.

Achieving something similar with Jenkins, Tekton or Argo Workflows simply isn't possible. You'd have to break one big pipeline up into lots of little ones. This might be in the best interests of small, manageable units, but the price is paid when trying to get visibility of the overall state of the system. I'd much rather look at one pipeline than fifteen to figure out why production is broken, and what has changed.

There is a very subtle implication of this. Concourse has no notion of a 'pipeline run'. Tekton and Argo Workflows notice a new source code version at the single point of entry, and then create a plan based on the current state of the pipeline, which is then executed.

By contrast, Concourse does not have this atomicity of pipeline executions, which has two consequences.

Firstly, if some jobs in a Concourse pipeline are triggered, and then another new resource triggers other jobs in the pipeline, these start running immediately. You can protect against this using the [pool resource](https://github.com/concourse/pool-resource) as a lock, and sometimes you _want_ to integrate changes as soon as possible.

Secondly, Jenkins, Tekton and Argo Workflows make it easy to see the shape of a pipeline for a given execution. Concourse does not, because in it there is no atomic pipeline execution.

I asked [Alex Suraci](https://www.linkedin.com/in/alex-suraci-62a64a1a1/), creator of Concourse whether this was some wise design choice in order to avoid bifurcations in causality. For instance, if Tekton allowed multiple entry points, and two pipelines executions occurred concurrently because of different triggers, causal history has forked - which of the resultant runs is the 'latest'? Presumably because Alex is rather smart, he said that it was a side effect rather than a conscious decision.

### Kubernetes is not required

You can run Concourse anywhere, which is great if you don't want to be tied to Kubernetes, or haven't adopted it yet. To be clear: you can absolutely run Concourse on Kubernetes if you want to, but Concourse doesn't _require_ it.

The downside of this is that Concourse attempts to do its own container scheduling, and doesn't always do a great job of it. We've not covered it in this series, but it's not uncommon to see snags where builds aren't scheduled as quickly as you'd like. It's certainly not as quick as Tekton and Argo Workflows, and if you get down-and-dirty debugging the internals of Concourse, you're going to need to know a lot of idiosyncratic things.

> ### Use Concourse when you:
>
> * want to run anywhere
> * have multiple reasons to trigger a pipeline
> * want a single view showing you the latest state of a pipeline
> * cannot have a CI system that can accept webhook HTTP requests from the public Internet (ie regulated environments)
> * want to easily run tasks on the CI server, without committing changes first
>
> ### Don't use Concourse if you
>
> * rely heavily on pull request-based workflows
> * have short, simple pipelines
> * have a low tolerance for investigating scheduling issues
> * want to only use Kubernetes tooling

## Conclusion of the conclusion

We started this comparison last year when working with Snyk on the [Continuous Delivery for Infrastructure-as-Code](https://resources.snyk.io/c/iac-book-continuous-?x=iiH-UX) book. We wanted to demonstrate those principles on all systems, so the practices could be adopted by the widest possible audience.

We were also interested in finding out how other tools compare to Concourse, which we've used a lot. There's no equivalent tool that is capable of doing the same things in the same way.

So, in conclusion...

Your life will probably be better if you don't use Jenkins.

Use Concourse if you're doing true continuous delivery, and find it desirable to have large integration pipelines that trigger for many reasons and present a snapshot view of the current state of a project/environment.

Use Tekton or Argo Workflows if you want many small, decoupled pipelines that are only triggered by a single Git repository, and are happy being tied to Kubernetes, and don't need a single build monitor view.
