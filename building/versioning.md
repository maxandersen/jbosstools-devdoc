DRAFT: This is a draft of how versioning are used in jboss tools projects - currently based on current "defacto" and orally agreed on versioning used in the related project(s)

## General versioning rules

JBoss projects in general follows the general naming at https://community.jboss.org/wiki/JBossProjectVersioning.

Plugins/features and even other components should be versioned according to http://wiki.eclipse.org/Version_Numbering

The rest of this document tries to cover what is not specified in these two documents and if there are exceptions to it.

Component - generic term used for something that is versioned. Could be a plugin, feature, target platform, product, updatesite etc.

Module - A component that includes a set of components, example: https://github.com/jbosstools/jbosstools-hibernate/ 

## Naming clarificiations

Both JBoss and osgi versioning rules say there are four parts to the versioning scheme.

JBoss name them: major.minor.micro.qualifier 

Eclipse names them: major.minor.service.qualifier

We consider these to be equal, i.e. micro==service. 

### Always use numerical option

JBoss rules state the numeric option on qualifiers are optional, i.e. Alpha is ok without a number.
We always add the number for consistency and that we know we are more than likely to have multiple of them.

The only qualifier that cannot have a numeric is Final. Final is final.

## Expected versioning sequence during development

When you do development you choose the right x.y.z for the next upcoming/planned release, i.e. 2.3.0.

A 2.3.0 that is all new and going to be used by other in-development components starts out by being named 2.3.0.Alpha1 and then progress through bumping the qualifier until you reach 2.3.0.Final.

Example:

    2.3.0.Alpha1
    2.3.0.Alpha2
    2.3.0.Beta1
    2.3.0.CR1
    2.3.0.Final

The exact number of alphas/betas/cr's might be planned for but reality might kicks in and add/subtract some during development; what is important that the qualifier keeps incrementing lexicographically to have a deterministic ordering for both humans and machines.

## When does a component go from Alpha to Beta to CR ?

The general rules (taken from JBoss description but with a clearer layout):

* Alpha - "for early releases that are not API complete, but have usuable new functional capabilities where the community can benefit from those new capabilites, plus contribute to the testing and fixing of those new capabilities."

* Beta - "for releases where the release is API complete, but its not completely tested, or there may even be questions about the API itself, that may change, even though the implementation is complete."

* CR (Candidate Release) - "for releases that we anticipate can be the GA (ed. Final) release, but we need the community to help validate the release."

Another important part is that your component should not be marked of higher quality than any of its dependencies. i.e. if you are using a dependency that is in Alpha you cannot be in CR1 since the subcomponent can/will change. 

## When does a component go Final ?

The simple rule is that a component goes Final once itself and all its subcomponent are Final.

This rule should be uphold and any deviance from it considered bad and spewed upon - and if it happens be clearly agreed and documented on which subpart has issues.

TODO: Examples from past?

## When does a component start increasing its micro ?

Once the release have gone final, i.e. 2.3.0.Final the qualifiers cannot "move" more for that micro release.
After this the component dependent on its development pase and phase do either a full development cycle:

    2.3.1.Alpha1 
    2.3.1.Alpha2 
    etc.

If the changes are pure and simple bugfixes and you got plenty of tests to verify this you can directly do

    2.3.1.Final

## What if I run out of numeric numbers, i.e. Alpha9 > Alpha10

If this ever occurs it most likely is because of too many broken releases and maybe using a SNAPSHOT mechanism would be better for your component.

In this case the option is to use "respin-qualifiers" to "extend" the numbers range, i.e. Alpha9a, Alpha9b etc.

TODO: is this the best solution ? 

## What if Final is released and there is a bug found ?

Example: you just release 2.3.0.Final and minutes/hours after a serious bug is found in your component or a subcomponent. What to do ?

Fix the bug/upgrade to the subcomponent that does not have the bug and version accordingly which hopefully would be just 2.3.1.Final.

In the worst case that the subcomponent needs to be upgraded to something that breaks/adds API you would need to do a 2.4.0 instead - but in that case you cannot just do a 2.4.0.Final directly. This would require a new development phase - thus should be avoided at all costs by getting the subcomponent fixed without breaking API.

## Bumping of versions across development streams

TODO: we must bump versions for anything rebuilt and released. In past we relied on timestamp increments to safe us - that breaks the basic rules in all the versioning guidelines - but we can't fix this without stopping rebuilding all the time OR have devs bumping versions all the time. 
Eclipse/OSGi uses +100 increments in service part. I think this is a good model to strive for, but has to be also clear that if your hard dependency changes from i.e. Juno to Kepler then just bumping service is wrong.
This is why I historically have had the opinion we just bump the minor version between dev stream - then there are no false promises.
 
