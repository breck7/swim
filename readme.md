# Swim - A Meta test Suite for Tree Notation Library Implementations

[Try Swim Language](http://jtree.treenotation.org/designer/#grammar%0A%20swimNode%0A%20%20description%20A%20language%20for%20writing%20tests%20without%20implementations%20that%20could%20be%20used%20by%20Tree%20Notation%20libraries.%0A%20%20root%0A%20%20inScope%20testNode%0A%20keywordCell%0A%20testNode%0A%20%20pattern%20Test%24%0A%20%20inScope%20arrangeNode%20actNode%20assertNode%0A%20%20cells%20keywordCell%0A%20anyCell%0A%20%20highlightScope%20string%0A%20contentNode%0A%20%20catchAllCellType%20anyCell%0A%20arrangeNode%0A%20%20catchAllNodeType%20contentNode%0A%20%20cells%20keywordCell%0A%20actNode%0A%20%20cells%20keywordCell%0A%20assertNode%0A%20%20catchAllNodeType%20contentNode%0A%20%20cells%20keywordCell%0Asample%0A%20aToStringTest%0A%20%20arrange%0A%20%20%20hello%0A%20%20%20%20world%0A%20%20act%0A%20%20assert%0A%20%20%20hello%0A%20%20%20%20world%0A%20aTotalLineCountTest%0A%20%20arrange%0A%20%20%20hello%0A%20%20%20%20world%0A%20%20act%0A%20%20assert%0A%20%20%202%0A%20aToCsvTest%0A%20%20arrange%0A%20%20%200%0A%20%20%20%20team%20ne%0A%20%20%20%20score%203%0A%20%20%201%0A%20%20%20%20team%20af%0A%20%20%20%20score%2028%0A%20%20act%0A%20%20assert%0A%20%20%20team%2Cscore%0A%20%20%20ne%2C3%0A%20%20%20af%2C28%0A%20aFromCsvTest%0A%20%20arrange%0A%20%20%20team%2Cscore%0A%20%20%20ne%2C3%0A%20%20%20af%2C28%0A%20%20act%0A%20%20assert%0A%20%20%200%0A%20%20%20%20team%20ne%0A%20%20%20%20score%203%0A%20%20%201%0A%20%20%20%20team%20af%0A%20%20%20%20score%2028%0A%20aGetTest%0A%20%20arrange%0A%20%20%20some%0A%20%20%20%20nested%0A%20%20%20%20%20node%0A%20%20%20%20%20%20color%20blue%0A%20%20act%0A%20%20assert%0A%20%20%20blue%0A%20aSetTest%0A%20%20arrange%0A%20%20%20message%0A%20%20%20%20hello%20world%0A%20%20act%0A%20%20assert%0A%20%20%20aloha%20world%0A%20aReverseTest%0A%20%20arrange%0A%20%20%20hello%0A%20%20%20world%0A%20%20act%0A%20%20assert%0A%20%20%20world%0A%20%20%20hello%0A%20aWordCountTest%0A%20%20arrange%0A%20%20%20hello%0A%20%20%20%20brave%0A%20%20%20%20%20new%20world%0A%20%20act%0A%20%20assert%0A%20%20%204%0A%20aFilterTest%0A%20%20arrange%0A%20%20%20keep%0A%20%20%20drop%0A%20%20%20keep%0A%20%20%20keep%0A%20%20act%0A%20%20assert%0A%20%20%20keep%0A%20%20%20keep%0A%20%20%20keep%0A%20aFromJsonSubsetTest%0A%20%20arrange%0A%20%20%20%7B%22message%22%3A%20%7B%22hello%22%3A%20%22world%22%7D%7D%0A%20%20act%0A%20%20assert%0A%20%20%20message%0A%20%20%20%20hello%20world)

Swim contains files with file extension "swim" that represent tests that a Tree Notation Library could implement.

Tree Notation is host language agnostic. You can build libraries for Tree Notation in languages from Ada to Zig and everything in between.

Although Tree Notation libraries should be designed to best meet the conventions and needs of the users of the host language, there are lots of benefits to be gained by coordinating with other Tree Notation libraries in other host languages when possible.

The Swim project is meant to serve as a useful checklist when implmenting a new Tree Notation library, as well as a place to discover and share good conventions that are applicable across host languages. Not all libraries need to implement all tests.

## Suggested Guidelines for Writing a New implementation

1. Keep a journal. Please document your process, particularly the annoying parts, so we can make it better! Bonus points for sharing your journal online.
2. Focus on serving the users of your host language first. If your language differs in a significant way from other host languages, don't worry too much about making your library look like the other Tree Notation libraries. Serve your users first.

## Suggested Usage of Swim

### Step 1. Understand the Swim language:

The general format of a swim file is as an array of tests, written like so:

    aTotalLineCountTest
     arrange
      hello
       world
     act
     assert
      2

The first line defines the name of the test. If a test fails, this generally is the title of the test your test runner would print.

The arrange node contains children that together form a string block that will be used as the setup for the test.

The act node is what you fill in -- put your implementation code here.

The assert node contains children that together from a string block that will contain the expected value of the test.

### Step 2. Copy the swim file to your repo and fill in the "act" portion of the tests with your implementation:

I added one line to the act node type in the example above with the TypeScript implementation (https://github.com/treenotation/jtree/blob/master/coreTests/core.swim):

    aTotalLineCountTest
     arrange
      hello
       world
     act
      new TreeNode(arrange).getNumberOfLines().toString()
     assert
      2

### Step 3. Build a test runner:

The Swim Test runner for the TypeScript/Javascript JTree implementation is short, only 23 lines of code, including setup code (https://github.com/treenotation/jtree/blob/master/coreTests/swim.test.ts). Here is the core piece:

    // Arrange/Act/Assert
    const tests = jtree.TreeNode.fromDisk(__dirname + "/core.swim")
    tests.forEach((test: any) => {
      const arrange = test.getNode("arrange").childrenToString()
      const expected = test.getNode("assert").childrenToString()
      const code = test.getNode("act").childrenToString()
      equal(eval(code), expected, test.getLine())
    })

Of course, the bulk of the work is in implementing a good Tree Notation library and not writing test runner code, but hopefully in your host language at least the runner code could be similarly short.
