- Why BEM? 
  - Less confusing, while still provides the good architecture we want and recognizable terminology. 
- [BEM naming rules](http://getbem.com/naming/)
- Block: Standalone entity that is meaningful on its own. While blocks can be nested and hierarchy and interact with each other, semantically they remain equal, there is no precedence or hierarchy. 
- Element: A part of block that has not its own standalone meaning and is semantically tied to its block. 
- Modifier: A flag on a block or element, used to change appearance or behavior. Add modifier classes only to blocks/elements they modify, and keep the origin classes. 
- Example for button component:
```
.button {
  ...
}
.button--state-success {
  ...
}
.button--state-danger {
  ...
}
```
- Why modifiers not represented as combined selector? [link](http://getbem.com/faq/#why-the-modifier-classes-are-prefixed)
  - Modifiers only affect on the blocks it belongs to
  - CSS specificity: combined classes are more specific, which may cause trouble when defining other combined/parent block classes. 
  - More readable
  - Easier when refactoring or global search 