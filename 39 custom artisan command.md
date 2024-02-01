{{
<?php

namespace App\Console\Commands;

use Illuminate\Console\Command;

class SendEventReminder extends Command
{
    
    protected $signature = 'app:send-event-reminder';
    protected $description = 'Command description';

    public function handle()
    {
        $events = \App\Models\Event::with('attendees.user')->where('start_time', '>=', now())
            ->where('start_time', '<=', now()->addHours(24))->get();

        $emails = [];
        foreach ($events as $event) {
            // $this->info($event->attendees);
            foreach($event->attendees as $attendee) {
                $emails[] = $attendee->user->email;
            }
        }

        $this->info(collect($emails));



    }
}

}}