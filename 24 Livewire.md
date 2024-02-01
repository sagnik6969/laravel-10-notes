##                               LIVEWIRE POLL APPLICATION

Livewire enables the dynamicity in our application without using Frontend frameworks like React,Vue etc

# ----> compulsory style & script  
{{
     @livewireStyles --> needed for livewire to work.
     @livewireScripts --> needed for livewire to work.

}}

# ----> livewire components

when we create a component using php artisan make:livewire component name 2 files get created
-> 1. app\http\livewire\file_name.php => contains all the logic
-> 2. resources\views\livewire\file_name.blade.php => contains html/css code.


# ----> use livewire magic inside component

{{
    <input type="text" wire:model="title" name="" id="">
    <p>Current Title{{ $title }}</p>
    <button wire:click="removeOption({{ $key }})" type="button">Remove</button>
    <button wire:click.prevent="addOption">Add option</button>

}}

--> $title => is a variable defined in app\http\livewire\file_name.php
--> unlike blade components we don't use construct inside livewire. 

# ----> to use a livewire component

{{
     @livewire('create-poll')
}}

## ---->

{{
   public function mount()
   -> similar to mount function of vue.js
   -> used to  initialize variables.
   

}}

# ----> livewire actions => just like vue methods 

\{\{
     public function createPoll()
    {
        $poll = Poll::create([
            'name' => $this->title
        ]);

        foreach ($this->options as $option) {

            $poll->options()->create([
                'name' => $option
            ]);
        }

        $this->reset(['title', 'options']);
        ----> for resting the form after the input is submitted
        
    }
}}

# <form wire:submit.prevent="createPoll">

# validation

{{
     protected $rules = [
        'title' => 'required|min:6',
        'options' => 'required|min:1|array',
        'options.*' => 'required'
        // For validating individual elements
    ];

    protected $messages = [
        'options.*' => 'The option cant be empty',
        // for custom error messages
    ];

}}

{{
    $this->validate();

      public function updated($propertyName)
    {
        $this->validateOnly($propertyName);
        // for live validation as we type in the input field
    }

}}

# ----> passing data to component

{{
     public function render()
    {
        $polls = Poll::with('options.votes')->latest()->get();
        // fetch both the options and votes
        return view('livewire.polls', [
            'polls' => $polls
        ]);
    }
}}


# ----> emitting events
\{{
     $this->emit('pollCreated');
}}

# listening for events

{{
    protected $listeners = [
        'pollCreated' => 'render'
    ];
}}