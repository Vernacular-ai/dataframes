# dataframes

Formats and schemata for data exchange at Vernacular.ai for Machine Learning
tasks. Here we define suggested ways of organizing data and labels for various
ML tasks that we work on day to day basis.

Two top level concepts are:
1. **Records**. Anything that can be considered as X, as per the ML terminology.
2. **Labels**. Things that can be considered as Y.

For connected systems, like an ASR pushing output to an Intent classifier, same
item can be considered as a record or label, depending on the usage pattern. The
schema definitions here will be pragmatic in blurring these boundaries in a way
that helps our most common workflows.

## Records
We define kinds of data by defining their protobuf definitions in
`./protos/records.proto`. The most common data points for us, and the ones we
are focusing on, are calls and turns. See messages `Call` and `Turn` for their
definitions. While there might be fields missing depending on deployment stage
and type, we stick to this format.

A `dataframe` is made up of a list of these `records`. You can have a static
dataframe which fixed set of records, or a dynamic dataframe which is a lazily
evaluated query giving you a list of records when you _ask_ for it. Common
example of a dynamic dataframe could be "100 random sample of calls from client
A from _last day_". You can also have a stream of records directly in real-time.

There are more fundamental data types like long and short audios, but we will
define their structures in a later version of this document.

## Labels
Similar to records, labels also have protobuf definitions which are kept in
`./protos/labels.proto`.

For any record, we can have multiple labels of different kinds. We organize
labels in layers which could have independent meaning which is left for
interpretation by the users. Few examples of why multiple layers are needed:

1. To separate different tagging types. For example you might want to tag the
   same turn for slot, intent, and transcription.
1. To separate prediction labels (made from a model) and human labels.
1. To separate multiple human labels. For consensus and subjective tests.
1. To separate different sets of same tagging types. For example separate label
   set for semantics and audio properties.
1. To separate versions of labels.

Recommended method is to keep these layers in separate files than records. See
the section on _Common Patterns_ for more details.

## Common Patterns
...
