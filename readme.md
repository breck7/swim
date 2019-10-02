# Swim - A Meta test Suite for Tree Notation Library Implementations

Swim contains files with file extension "swim" that represent tests that a Tree Notation Library could implement.

Tree Notation is host language agnostic. You can build libraries for Tree Notation in languages from Ada to Zig and everything in between.

Although Tree Notation libraries should be designed to best meet the conventions and needs of the users of the host language, there are lots of benefits to be gained by coordinating with other Tree Notation libraries in other host languages when possible.

The Swim project is meant to serve as a useful checklist when implmenting a new Tree Notation library, as well as a place to discover and share good conventions that are applicable across host languages. Not all libraries need to implement all tests.

## Using

The general format of a swim file is as an array of tests, written like so:

    descriptionOfATotalLineCountTestInCamelCase
     arrange
      the tree structure goes here
     act
     assert
      number 1

Your library might run such a test suite in a manner similar to this pseudocode:

    tests = getSwimTests()
    tests.descriptionOfATotalLineCountTestInCamelCase.act = tree => tree.length
    tests.forEach(test => {
      if !tests.act
       skip # only run tests that you have defined the act steps for
      tree = new tree(test.arrange)
      assert test.act(tree) === test.assert
    })
